# [LeetCode329.矩阵中的最长递增路径](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/)
## 题目描述
给定一个整数矩阵，找出最长递增路径的长度。

对于每个单元格，你可以往上，下，左，右四个方向移动。 你不能在对角线方向上移动或移动到边界外（即不允许环绕）。

### 示例
```
输入: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
输出: 4 
解释: 最长递增路径为 [1, 2, 6, 9]。
```
```
输入: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
输出: 4 
解释: 最长递增路径是 [3, 4, 5, 6]。注意不允许在对角线方向上移动。
```
## 题解
### 解法一
`DFS`+记忆化搜索:

将矩阵看成一个有向图，每个单元格对应图中的一个节点，如果相邻的两个单元格的值不相等，则在相邻的两个单元格之间存在一条从较小值指向较大值的有向边。问题转化成在有向图中寻找最长路径。

深度优先搜索是非常直观的方法。从一个单元格开始进行深度优先搜索，即可找到从该单元格开始的最长递增路径。对每个单元格分别进行深度优先搜索之后，即可得到矩阵中的最长递增路径的长度。

但是如果使用朴素深度优先搜索，时间复杂度是指数级，会超出时间限制，因此必须加以优化。

朴素深度优先搜索的时间复杂度过高的原因是进行了大量的重复计算，同一个单元格会被访问多次，每次访问都要重新计算。由于同一个单元格对应的最长递增路径的长度是固定不变的，因此可以使用记忆化的方法进行优化。用矩阵 `memo` 作为缓存矩阵，已经计算过的单元格的结果存储到缓存矩阵中。

使用记忆化深度优先搜索，当访问到一个单元格 `(i,j)` 时，如果 `memo[i][j]≠0`，说明该单元格的结果已经计算过，则直接从缓存中读取结果，如果 `memo[i][j]=0`，说明该单元格的结果尚未被计算过，则进行搜索，并将计算得到的结果存入缓存中。

遍历完矩阵中的所有单元格之后，即可得到矩阵中的最长递增路径的长度。

纯`DFS`会超时:
```java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return 0;
        }
        int ans=0;
        int m=matrix.length,n=matrix[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                ans=Math.max(ans,dfs(matrix,i,j));
            }
        }
        return ans;
    }
    public int dfs(int[][] matrix,int i,int j){
        int[][] dirs={{-1,0},{1,0},{0,-1},{0,1}};
        int res=1;
        for(int[] dir:dirs){
            int newI=i+dir[0],newJ=j+dir[1];
            if(newI>=0&&newI<matrix.length&&newJ>=0&&newJ<matrix[0].length&&matrix[newI][newJ]>matrix[i][j]){
                res=Math.max(res,dfs(matrix,newI,newJ)+1);
            }
        }
        return res;
    }
}
```
加入记忆化搜索：
```java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return 0;
        }
        int ans=0;
        int m=matrix.length,n=matrix[0].length;
        int[][] memo=new int[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                ans=Math.max(ans,dfs(matrix,i,j,memo));
            }
        }
        return ans;
    }
    public int dfs(int[][] matrix,int i,int j,int[][] memo){
        if(memo[i][j]!=0){
            return memo[i][j];
        }
        int[][] dirs={{-1,0},{1,0},{0,-1},{0,1}};
        memo[i][j]=1;
        for(int[] dir:dirs){
            int newI=i+dir[0],newJ=j+dir[1];
            if(newI>=0&&newI<matrix.length&&newJ>=0&&newJ<matrix[0].length&&matrix[newI][newJ]>matrix[i][j]){
                memo[i][j]=Math.max(memo[i][j],dfs(matrix,newI,newJ,memo)+1);
            }
        }
        return memo[i][j];
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mn)$，其中 `m` 和 `n` 分别是矩阵的行数和列数。深度优先搜索的时间复杂度是 $O(V+E)$，其中 `V` 是节点数，`E` 是边数。在矩阵中，$O(V)=O(mn)$，$O(E)≈O(4mn)=O(mn)$。
- 空间复杂度：$O(mn)$，其中 `m` 和 `n` 分别是矩阵的行数和列数。空间复杂度主要取决于缓存和递归调用深度，缓存的空间复杂度是 $O(mn)$，递归调用深度不会超过 `mn`。
### 解法二
拓扑排序

将矩阵看成一个有向图，计算每个单元格对应的出度，即有多少条边从该单元格出发。对于作为边界条件的单元格，该单元格的值比所有的相邻单元格的值都要大。

基于出度的概念，可以使用拓扑排序求解。从所有出度为 0 的单元格开始广度优先搜索，每一轮搜索都会遍历当前层的所有单元格，更新其余单元格的出度，并将出度变为 0 的单元格加入下一层搜索。当搜索结束时，搜索的总层数即为矩阵中的最长递增路径的长度。

```java
class Solution {
    public int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    public int rows, columns;

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        rows = matrix.length;
        columns = matrix[0].length;
        int[][] outdegrees = new int[rows][columns];
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                for (int[] dir : dirs) {
                    int newRow = i + dir[0], newColumn = j + dir[1];
                    if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] > matrix[i][j]) {
                        ++outdegrees[i][j];
                    }
                }
            }
        }
        Queue<int[]> queue = new LinkedList<int[]>();
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                if (outdegrees[i][j] == 0) {
                    queue.offer(new int[]{i, j});
                }
            }
        }
        int ans = 0;
        while (!queue.isEmpty()) {
            ++ans;
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] cell = queue.poll();
                int row = cell[0], column = cell[1];
                for (int[] dir : dirs) {
                    int newRow = row + dir[0], newColumn = column + dir[1];
                    if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] < matrix[row][column]) {
                        --outdegrees[newRow][newColumn];
                        if (outdegrees[newRow][newColumn] == 0) {
                            queue.offer(new int[]{newRow, newColumn});
                        }
                    }
                }
            }
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mn)$，其中 `m` 和 `n` 分别是矩阵的行数和列数。拓扑排序的时间复杂度是 $O(V+E)$，其中 `V` 是节点数，`E` 是边数。在矩阵中，$O(V)=O(mn)$，$O(E)≈O(4mn)=O(mn)$。
- 空间复杂度：$O(mn)$，其中 `m` 和 `n` 分别是矩阵的行数和列数。空间复杂度主要取决于队列，队列的空间复杂度是 $O(mn)$。