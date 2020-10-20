# [LeetCode130.被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)
## 题目描述
给定一个二维的矩阵，包含 `'X'` 和 `'O'`（字母 `O`）。

找到所有被 `'X'` 围绕的区域，并将这些区域里所有的 `'O'` 用 `'X'` 填充。

示例:
```
X X X X
X O O X
X X O X
X O X X
```
运行你的函数后，矩阵变为：
```
X X X X
X X X X
X X X X
X O X X
```
解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 `'O'` 都不会被填充为 `'X'`。 任何不在边界上，或不与边界上的 `'O'` 相连的 `'O'` 最终都会被填充为`'X'`。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

## 题解
### 解法一
本题给定的矩阵中有三种元素：

- 字母 `X`；
- 被字母 `X` 包围的字母 `O`；
- 没有被字母 `X` 包围的字母 `O`。
本题要求将所有被字母 `X` 包围的字母 `O`都变为字母 `X` ，但很难判断哪些 `O` 是被包围的，哪些 `O` 不是被包围的。

注意到题目解释中提到：任何边界上的 `O` 都不会被填充为 `X`。 我们可以想到，所有的不被包围的 `O` 都直接或间接与边界上的 `O` 相连。我们可以利用这个性质判断 `O` 是否在边界上，具体地说：

- 对于每一个边界上的 `O`，我们以它为起点，标记所有与它直接或间接相连的字母 `O`；
- 最后我们遍历这个矩阵，对于每一个字母：
  - 如果该字母被标记过，则该字母为没有被字母 `X` 包围的字母 `O`，我们将其还原为字母 `O`；
  - 如果该字母没有被标记过，则该字母为被字母 `X` 包围的字母 `O`，我们将其修改为字母 `X`。

`DFS`:
```java
class Solution {
    public void solve(char[][] board) {
        if(board.length==0||board[0].length==0){
            return;
        }
        for(int i=0;i<board[0].length;i++){
            dfs(board,0,i);
            dfs(board,board.length-1,i);
        }
        for(int i=0;i<board.length;i++){
            dfs(board,i,0);
            dfs(board,i,board[0].length-1);
        }
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                if(board[i][j]=='O'){
                    board[i][j]='X';
                }else if(board[i][j]=='A'){
                    board[i][j]='O';
                }
            }
        }
        return;
    }
    public void dfs(char[][] board,int i,int j){
        if(i<0||i>board.length-1||j<0||j>board[0].length-1||board[i][j]!='O'){
            return;
        }
        board[i][j]='A';
        dfs(board,i-1,j);
        dfs(board,i+1,j);
        dfs(board,i,j-1);
        dfs(board,i,j+1);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n×m)$，其中 `n` 和 `m` 分别为矩阵的行数和列数。深度优先搜索过程中，每一个点至多只会被标记一次。
- 空间复杂度：$O(n×m)$，其中 `n` 和 `m` 分别为矩阵的行数和列数。主要为深度优先搜索的栈的开销。
### 解法二
`BFS`:

```java
class Solution {
    public void solve(char[][] board) {
        if(board.length==0||board[0].length==0){
            return;
        }
        for(int i=0;i<board[0].length;i++){
            if(board[0][i]=='O'){
                bfs(board,0,i);
            }
            if(board[board.length-1][i]=='O'){
                bfs(board,board.length-1,i);
            }
        }
        for(int i=0;i<board.length;i++){
            if(board[i][0]=='O'){
                bfs(board,i,0);
            }
            if(board[i][board[0].length-1]=='O'){
                bfs(board,i,board[0].length-1);
            } 
        }
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                if(board[i][j]=='O'){
                    board[i][j]='X';
                }else if(board[i][j]=='A'){
                    board[i][j]='O';
                }
            }
        }
        return;
    }
    public void bfs(char[][] board,int i,int j){
        Queue<int[]> queue=new LinkedList<>();
        queue.offer(new int[]{i,j});
        while(!queue.isEmpty()){
            int[] temp=queue.poll();
            int curX=temp[0],curY=temp[1];
            if(curX>=0&&curX<board.length&&curY>=0&&curY<board[0].length&&board[curX][curY]=='O'){
                board[curX][curY]='A';
                queue.offer(new int[]{curX-1,curY});
                queue.offer(new int[]{curX+1,curY});
                queue.offer(new int[]{curX,curY-1});
                queue.offer(new int[]{curX,curY+1});
            }
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n×m)$，其中 `n` 和 `m` 分别为矩阵的行数和列数。广度优先搜索过程中，每一个点至多只会被标记一次。
- 空间复杂度：$O(n×m)$，其中 `n` 和 `m` 分别为矩阵的行数和列数。主要为广度优先搜索的队列的开销。
