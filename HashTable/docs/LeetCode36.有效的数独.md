# [LeetCode36.有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)
## 题目描述
判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。

- 数字 1-9 在每一行只能出现一次。
- 数字 1-9 在每一列只能出现一次。
- 数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201029211709.png)

上图是一个部分填充的有效的数独。

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

说明:

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。
- 给定数独序列只包含数字 1-9 和字符 `'.'` 。
- 给定数独永远是 9x9 形式的。

### 示例
```
输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```
```
输入:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```
## 题解
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int[][] col=new int[9][9];
        int[][] square=new int[9][9];
        for(int i=0;i<9;i++){
            int[] row=new int[9];
            for(int j=0;j<9;j++){
                if(board[i][j]!='.'){
                    if(row[board[i][j]-'1']!=0){
                        return false;
                    }else{
                        row[board[i][j]-'1']++;
                    }
                    if(col[j][board[i][j]-'1']!=0){
                        return false;
                    }else{
                        col[j][board[i][j]-'1']++;
                    }
                    int idx=3*(i/3)+j/3;
                    if(square[idx][board[i][j]-'1']!=0){
                        return false;
                    }else{
                        square[idx][board[i][j]-'1']++;
                    }
                }
                
            }
        }
        return true;
    }
}
```
或
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        List<HashSet<Character>> col=new ArrayList<>();
        for(int i=0;i<9;i++){
            col.add(new HashSet<Character>());
        }
        List<HashSet<Character>> square=new ArrayList<>();
        for(int i=0;i<9;i++){
            square.add(new HashSet<Character>());
        }
        for(int i=0;i<9;i++){
            HashSet<Character> row=new HashSet<>();
            for(int j=0;j<9;j++){
                if(board[i][j]!='.'){
                    if(row.contains(board[i][j])){
                        return false;
                    }else{
                        row.add(board[i][j]);
                    }
                    HashSet tempCol=col.get(j);
                    if(tempCol.contains(board[i][j])){
                        return false;
                    }else{
                        tempCol.add(board[i][j]);
                    }
                    int idx=3*(i/3)+j/3;
                    HashSet tempSquare=square.get(idx);
                    if(tempSquare.contains(board[i][j])){
                        return false;
                    }else{
                        tempSquare.add(board[i][j]);
                    }
                }
                
            }
        }
        return true;
    }
}
```
## 复杂度分析
- 时间复杂度：$O(1)$，因为我们只对 81 个单元格进行了一次迭代。
- 空间复杂度：$O(1)$。