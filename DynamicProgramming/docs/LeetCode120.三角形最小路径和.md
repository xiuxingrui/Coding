# [LeetCode120.三角形最小路径和](https://leetcode-cn.com/problems/triangle/)
## 题目描述
给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。

例如，给定三角形：

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

如果你可以只使用 $O(n)$ 的额外空间（`n` 为三角形的总行数）来解决这个问题，那么你的算法会很加分。

## 题解
### 解法一
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int size=triangle.size();
        int[][] dp=new int[size+1][size+1];
        for(int i=size-1;i>=0;i--){
            for(int j=0;j<i+1;j++){
                dp[i][j]=Math.min(dp[i+1][j],dp[i+1][j+1])+triangle.get(i).get(j);
            }
        }
        return dp[0][0];
    }
}
```
优化空间复杂度:
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] dp = new int[n + 1];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i).get(j);
            }
        }
        return dp[0];
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N^2)$，`N` 为三角形的行数。
- 空间复杂度：$O(N^2)$，`N` 为三角形的行数。优化后$O(N)$。
### 解法二
若定义 `f(i,j)` 点到底边的最小路径和，则易知递归求解式为:

```
f(i,j)=min(f(i+1,j),f(i+1,j+1))+triangle[i][j]
```

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        return dfs(triangle,0,0);
    }
    public int dfs(List<List<Integer>> triangle,int i,int j){
        if(i==triangle.size()){
            return 0;
        }
        return Math.min(dfs(triangle,i+1,j),dfs(triangle,i+1,j+1))+triangle.get(i).get(j);
    }
}
```
暴力搜索会有大量的重复计算，导致 超时，因此结合记忆化数组进行优化。
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        Integer[][] memo = new Integer[triangle.size()][triangle.size()];
        return  dfs(triangle, 0, 0,memo);
    }

    private int dfs(List<List<Integer>> triangle, int i, int j,Integer[][] memo) {
        if (i == triangle.size()) {
            return 0;
        }
        if (memo[i][j] !=null) {
            return memo[i][j];
        }
        return memo[i][j] = Math.min(dfs(triangle, i + 1, j,memo), dfs(triangle, i + 1, j + 1,memo)) + triangle.get(i).get(j);
    }
}
```
#### 复杂度分析
优化后:
- 时间复杂度：$O(N^2)$，`N` 为三角形的行数。
- 空间复杂度：$O(N^2)$，`N` 为三角形的行数。