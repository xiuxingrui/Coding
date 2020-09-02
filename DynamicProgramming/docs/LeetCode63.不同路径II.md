# [LeetCode63.不同路径II](https://leetcode-cn.com/problems/unique-paths-ii/)
## 题目描述
一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为`“Start”` ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为`“Finish”`）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200902121652.png)


网格中的障碍物和空位置分别用 1 和 0 来表示。

说明：`m` 和 `n`的值均不超过 100。
### 示例
```
输入:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
输出: 2
解释:
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```
## 题解
### 解法一
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        if (obstacleGrid == null || obstacleGrid.length == 0) {
            return 0;
        }
        
        // 定义 dp 数组并初始化第 1 行和第 1 列。
        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];
        for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) {
            dp[i][0] = 1;
        }
        for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++) {
            dp[0][j] = 1;
        }

        // 根据状态转移方程 dp[i][j] = dp[i - 1][j] + dp[i][j - 1] 进行递推。
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
空间优化:
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int n = obstacleGrid.length, m = obstacleGrid[0].length;
        int[] f = new int[m];

        f[0] = obstacleGrid[0][0] == 0 ? 1 : 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (obstacleGrid[i][j] == 1) {
                    f[j] = 0;
                    continue;
                }
                if (j - 1 >= 0 && obstacleGrid[i][j - 1] == 0) {
                    f[j] += f[j - 1];
                }
            }
        }
        
        return f[m - 1];
    }
}
```
自己写法:
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m=obstacleGrid.length;
        int n=obstacleGrid[0].length;
        int[][] dp=new int[m+1][n+1];
        dp[0][1]=1;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(obstacleGrid[i-1][j-1]!=1){
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];
                }
            }
        }
        return dp[m][n];
    }
}
```
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m=obstacleGrid.length;
        int n=obstacleGrid[0].length;
        int[] dp=new int[n+1];
        dp[0]=1;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(i>1){
                    dp[0]=0;
                }
                if(obstacleGrid[i-1][j-1]==1){
                    dp[j]=0;
                }else{
                    dp[j]=dp[j]+dp[j-1];
                }
            }
        }
        return dp[n];
    }
}
```
### 解法二
BFS超时
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m=obstacleGrid.length;
        int n=obstacleGrid[0].length;
        Queue<Pair<Integer,Integer>> queue=new LinkedList<>();
        int count=0;
        if(obstacleGrid[0][0]!=1){
            queue.offer(new Pair<Integer,Integer>(1,1));
        }
        while(!queue.isEmpty()){
            Pair<Integer,Integer> temp=queue.poll();
            int i=temp.getKey();
            int j=temp.getValue();
            if(i==m&&j==n){
                count++;
            }
            if(i<m&&obstacleGrid[i][j-1]!=1){
                queue.offer(new Pair<Integer,Integer>(i+1,j));
            }
            if(j<n&&obstacleGrid[i-1][j]!=1){
                queue.offer(new Pair<Integer,Integer>(i,j+1));
            }
        }
        return count;
    }
}
```