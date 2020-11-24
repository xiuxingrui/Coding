# [LeetCode576.出界的路径数](https://leetcode-cn.com/problems/out-of-boundary-paths/)
## 题目描述
给定一个 `m × n` 的网格和一个球。球的起始坐标为 `(i,j)` ，你可以将球移到相邻的单元格内，或者往上、下、左、右四个方向上移动使球穿过网格边界。但是，你最多可以移动 `N` 次。找出可以将球移出边界的路径数量。答案可能非常大，返回 结果 `mod 109 + 7` 的值。

### 示例
```
输入: m = 2, n = 2, N = 2, i = 0, j = 0
输出: 6
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201123171720.png)

```
输入: m = 1, n = 3, N = 3, i = 0, j = 1
输出: 12
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201123171731.png)

## 题解
`dp[i][j][k]`:表示从`(i,j)`出发第`k`步出界的路径总数，等价于从外界出发第`k`步走到`(i,j)`的路径总数

显然我们可以直接获得如下状态转移方程

`dp[i][j][k]=dp[i−1][j][k−1]+dp[i+1][j][k−1]+dp[i][j−1][k−1]+dp[i][j+1][k−1]`

初始化：我们需要注意外界的坐标的初始状态对应的值为1
```java
class Solution {
    public int findPaths(int m, int n, int N, int i, int j) {
        // dp[i][j][k]:表示从(i,j)出发第k步出界的路径总数，等价于从外界出发第k步走到(i,j)的路径总数
        long[][][] dp = new long[m + 2][n + 2][N + 1];
        int mod = 1000000007;
        if (N == 0) {
            return 0;
        }
        for (int r = 0; r <= m + 1; r++) {
            // 第 0 列和第 n + 1 列
            dp[r][0][0] = 1;
            dp[r][n + 1][0] = 1;
        }
        for (int c = 0; c <= n + 1; c++) {
            // 第 0 行和 第 m + 1 行
            dp[0][c][0] = 1;
            dp[m + 1][c][0] = 1;
        }

        for (int k = 1; k <= N; k++) {
            for (int r = 1; r <= m; r++) {
                for (int c = 1; c <= n; c++) {
                    dp[r][c][k] = (dp[r - 1][c][k - 1] + dp[r + 1][c][k - 1] + dp[r][c - 1][k - 1]
                                    + dp[r][c + 1][k - 1]) % mod;
                }
            }
        }
        long ans = 0;
        // 的结果加起来
        for (int k = 1; k <= N; k++) {
            ans = (ans + dp[i + 1][j + 1][k]) % mod;
        }
        return (int)ans;
    }
}
```
`DFS`超时，时间复杂度：$O(4^n)$，递归树的大小将为 `4^n`。这里，`n` 表示允许的移动次数。空间复杂度：$O(n)$，递归树的深度可以达到 `n`。
```java
class Solution {
    int cnt=0;
    public int findPaths(int m, int n, int N, int i, int j) {
        dfs(m,n,N,i,j,0);
        return cnt;
    }
    public void dfs(int m,int n,int N,int i,int j,int step){
        if(step>N){
            return;
        }
        if(i<0||i>=m||j<0||j>=n){
            cnt++;
            return;
        }
        int[][] dirs={{0,1},{0,-1},{1,0},{-1,0}};
        for(int k=0;k<4;k++){
            int newI=i+dirs[k][0],newJ=j+dirs[k][1];
            dfs(m,n,N,newI,newJ,step+1);
        }
    }
}
```

 