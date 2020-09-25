# [剑指Offer12.矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
## 题目描述
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串`“bfce”`的路径（路径中的字母用加粗标出）。

```
[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]
```
但矩阵中不包含字符串`“abfb”`的路径，因为字符串的第一个字符`b`占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

- `1 <= board.length <= 200`
- `1 <= board[i].length <= 200`

### 示例
```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```
```
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```
## 题解

在`DFS`的时候，如果已经找到一个正确的路径了，换句话说已经得到结果了，其实就没必要继续`DFS`了，直接返回结果即可。本题必须剪枝处理，否则会超时。
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        if(board==null||board.length==0||board[0].length==0){
            return false;
        }
        char[] chs=word.toCharArray();
        int m=board.length,n=board[0].length;
        boolean[][] visited=new boolean[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]==chs[0]){
                    if(backtrack(board,chs,0,i,j,visited)){
                        return true;
                    }
                }
            }
        }
        return false;
    }
    public boolean backtrack(char[][] board,char[] chs,int len,int i,int j,boolean[][] visited){
        if(len==chs.length){
            return true;
        }
        if(i<0||i>=board.length||j<0||j>=board[0].length||visited[i][j]||board[i][j]!=chs[len]){
            return false;
        }
        visited[i][j]=true;
        boolean flag=backtrack(board,chs,len+1,i-1,j,visited)||backtrack(board,chs,len+1,i+1,j,visited)||backtrack(board,chs,len+1,i,j-1,visited)||backtrack(board,chs,len+1,i,j+1,visited);
        visited[i][j]=false;
        return flag;
    }
}
```
或
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int m=board.length,n=board[0].length;
        boolean[][] visited=new boolean[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]==word.charAt(0)){
                    if(backtrack(board,word,word.length(),i,j,visited,0)){
                        return true;
                    }
                }
            }
        }
        return false;
    }
    public boolean backtrack(char[][] board,String word,int len,int i,int j,boolean[][] visited,int k){
        if(len==0){
            return true;
        }
        if(i<0||i>=board.length||j<0||j>=board[0].length||visited[i][j]==true||board[i][j]!=word.charAt(k)){
            return false;
        }
        visited[i][j]=true;
        if (backtrack(board,word,len-1,i-1,j,visited,k+1)||backtrack(board,word,len-1,i+1,j,visited,k+1)||backtrack(board,word,len-1,i,j-1,visited,k+1)||backtrack(board,word,len-1,i,j+1,visited,k+1)){
            return true;
        }
        visited[i][j]=false;
        return false;
    }
}
```
或者
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int m=board.length,n=board[0].length;
        boolean[][] visited=new boolean[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(backtrack(board,word,0,i,j,visited)){
                    return true;
                }
            }
        }
        return false;
    }
    public boolean backtrack(char[][] board,String word,int idx,int i,int j,boolean[][] visited){
        if(i<0||i>=board.length||j<0||j>=board[0].length||visited[i][j]==true||board[i][j]!=word.charAt(idx)){
            return false;
        }
        if(idx==word.length()-1){
            return true;
        }
        visited[i][j]=true;
        if (backtrack(board,word,idx+1,i-1,j,visited)||backtrack(board,word,idx+1,i+1,j,visited)||backtrack(board,word,idx+1,i,j-1,visited)||backtrack(board,word,idx+1,i,j+1,visited)){
            return true;
        }
        visited[i][j]=false;
        return false;
    }
}
```

### 复杂度分析

- 时间复杂度：一个非常宽松的上界为 $O(M*N*3^{L})$，其中 `M,N`为网格的长度与宽度，`L`为字符串 `word` 的长度。在每次调用回溯函数时，除了第一次可以进入 4 个分支以外，其余时间我们最多会进入 3 个分支（因为每个位置只能使用一次，所以走过来的分支没法走回去）。由于单词长为 `L`，故回溯函数的时间复杂度为 $O(3^L)$，而我们要执行 $O(MN)$ 次检查。然而，由于剪枝的存在，我们在遇到不匹配或已访问的字符时会提前退出，终止递归流程。因此，实际的时间复杂度会远远小于$O(M*N*3^{L})$。

- 空间复杂度：$O(MN)$。我们额外开辟了 $O(MN)$ 的 `visited` 数组，同时栈的深度最大为 $O(min(L,MN))$。