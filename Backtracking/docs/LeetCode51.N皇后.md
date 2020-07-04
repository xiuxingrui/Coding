# [LeetCode51.N皇后](https://leetcode-cn.com/problems/n-queens/)
## 题目描述
`n` 皇后问题研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。皇后可以攻击同一行、同一列、左上左下右上右下四个方向的任意单位。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200704211515.png)

给定一个整数 `n`，返回所有不同的 `n` 皇后问题的解决方案。

每一种解法包含一个明确的 `n` 皇后问题的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

### 示例
```
输入: 4
输出: [
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。
```
## 题解
### 解法一:
```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> res=new ArrayList<>();
        ArrayList<Integer> path=new ArrayList<>();
        int[] nums=new int[n];
        for(int i=0;i<n;++i){
            nums[i]=i;
        }
        boolean[] used=new boolean[n];
        helper(res,path,nums,used,0,n);
        return res;
    }
    public void helper(List<List<String>> res,ArrayList<Integer> path,int[] nums,boolean[] used,int p,int n){
        if(path.size()==n){
            LinkedList<String> str=new LinkedList<>();
            for(int i=0;i<n;++i){
                StringBuilder sb=new StringBuilder();
                int pos=path.get(i);
                for(int j=0;j<n;++j){
                    if(j==pos){
                        sb.append('Q');
                    }
                    else sb.append('.');
                }
                str.add(sb.toString());
            }
            res.add(str);
        }
        for(int k=0;k<n;++k){
            if(used[k]) continue;
            boolean is=true;
            for(int l=0;l<p;++l){
                if(Math.abs(p-l)==Math.abs(nums[k]-path.get(l))){
                    is=false;
                    break;
                }
            }
            if(is==true){
                used[k]=true;
                path.add(nums[k]);
                helper(res,path,nums,used,p+1,n);
                path.remove(p);
                used[k]=false;
            }
        }
    }
}
```
#### 复杂度分析
- 时间复杂度:$O(n!)$
- 空间复杂度:$O(n)$
### 解法二:
```java
class Solution{

        List<List<String>> res;

        public List<List<String>> solveNQueens(int n) {
            if (n <= 0) return null;
            res = new LinkedList<>();
            char[][] board = new char[n][n];
            for (char[] chars : board) Arrays.fill(chars, '.');
            backtrack(board, 0);
            return res;
        }

        /**
         * 路径：board中小于row的那些行都已经成功放置了皇后
         * 可选择列表: 第row行的所有列都是放置Q的选择
         * 结束条件: row超过board的最后一行
         *
         * @param board
         * @param row
         */
        private void backtrack(char[][] board, int row) {
            if (row == board.length) {
                res.add(charToString(board));
                return;
            }
            int n = board[row].length;
            for (int col = 0; col < n; col++) {
                if (!isValid(board, row, col)) continue;
                board[row][col] = 'Q';
                backtrack(board, row + 1);
                board[row][col] = '.';
            }
        }
        /* 是否可以在 board[row][col] 放置皇后？ */
        private boolean isValid(char[][] board, int row, int col) {
            int rows = board.length;
            // 检查列是否有皇后互相冲突
            for (char[] chars : board) if (chars[col] == 'Q') return false;
            // 检查右上方是否有皇后互相冲突
            for (int i = row - 1, j = col + 1; i >= 0 && j < rows; i--, j++) {
                if (board[i][j] == 'Q') return false;
            }
            // 检查左上方是否有皇后互相冲突
            for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
                if (board[i][j] == 'Q') return false;
            }
            return true;
        }
        private static List<String> charToString(char[][] array) {
            List<String> result = new LinkedList<>();
            for (char[] chars : array) {
                result.add(String.valueOf(chars));
            }
            return result;
        }
    }
```
#### 复杂度分析
- 时间复杂度:$O(n!)$
- 空间复杂度:$O(n)$