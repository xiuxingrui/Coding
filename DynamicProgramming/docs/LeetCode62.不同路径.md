# [LeetCode62.不同路径](https://leetcode-cn.com/problems/unique-paths/)
## 题目描述
一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为`“Start”` ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为`“Finish”`）。

问总共有多少条不同的路径？

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200902111624.png)

例如，上图是一个7 x 3 的网格。有多少可能的路径？?

- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 10 ^ 9`

### 示例
```
输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
```
```
输入: m = 7, n = 3
输出: 28
```
## 题解
### 解法一
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for (int i = 0; i < n; i++) dp[0][i] = 1;
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];  
    }
}
```
优化存储空间：
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] cur = new int[n];
        Arrays.fill(cur,1);
        for (int i = 1; i < m;i++){
            for (int j = 1; j < n; j++){
                cur[j] =cur[j]+cur[j-1] ;
            }
        }
        return cur[n-1];
    }
}
```
自己写法:
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp=new int[m+1][n+1];
        dp[0][1]=1;//dp[1][0]=1;也行，只要保证dp[1][1]=1
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m][n];
    }
}
```
空间优化:
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp=new int[n];
        dp[0]=1;
        for(int i=1;i<=m;i++){
            for(int j=1;j<n;j++){
                dp[j]=dp[j]+dp[j-1];
            }
        }
        return dp[n-1];
    }
}
```
### 解法二
BFS:超时
```java
class Solution {
    public int uniquePaths(int m, int n) {
        Queue<Pair<Integer,Integer>> queue=new LinkedList<>();
        int count=0;
        queue.offer(new Pair<Integer,Integer>(1,1));
        while(!queue.isEmpty()){
            Pair<Integer,Integer> temp=queue.poll();
            int i=temp.getKey();
            int j=temp.getValue();
            if(i==m&&j==n){
                count++;
            }
            if(i<m){
                queue.offer(new Pair<Integer,Integer>(i+1,j));
            }
            if(j<n){
                queue.offer(new Pair<Integer,Integer>(i,j+1));
            }
        }
        return count;
    }
}
```