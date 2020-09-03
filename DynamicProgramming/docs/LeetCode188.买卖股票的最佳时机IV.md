# [LeetCode188.买卖股票的最佳时机IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)
## 题目描述
给定一个数组，它的第 `i` 个元素是一支给定的股票在第 `i` 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 `k` 笔交易。

注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

### 示例
```
输入: [2,4,1], k = 2
输出: 2
解释: 在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```
```
输入: [3,2,6,5,0,3], k = 2
输出: 7
解释: 在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```
## 题解
超出内存限制：
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if(prices==null||prices.length==0){
            return 0;
        }
        int len=prices.length;
        int[][][] dp=new int[len][k+1][2];
        for(int i=0;i<len;i++){
            dp[i][0][0]=0;
            dp[i][0][1]=Integer.MIN_VALUE;
        }
        for(int i=1;i<=k;i++){
            dp[0][i][0]=0;
            dp[0][i][1]=-prices[0];
        }
        for(int i=1;i<len;i++){
            for(int j=1;j<=k;j++){
                dp[i][j][0]=Math.max(dp[i-1][j][0],dp[i-1][j][1]+prices[i]);
                dp[i][j][1]=Math.max(dp[i-1][j][1],dp[i-1][j-1][0]-prices[i]);
            }
        }
        return dp[len-1][k][0];
    }
}
```
优化存储空间
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if(prices==null||prices.length==0){
            return 0;
        }
        int len=prices.length;
        int[][] dp=new int[k+1][2];
        dp[0][0]=0;
        dp[0][1]=Integer.MIN_VALUE;
        for(int i=1;i<=k;i++){
            dp[i][0]=0;
            dp[i][1]=-prices[0];
        }
        for(int i=1;i<len;i++){
            for(int j=1;j<=k;j++){
                dp[j][0]=Math.max(dp[j][0],dp[j][1]+prices[i]);
                dp[j][1]=Math.max(dp[j][1],dp[j-1][0]-prices[i]);
            }
        }
        return dp[k][0];
    }
}
```s
一次交易由买入和卖出构成，至少需要两天。所以说有效的限制 `k` 应该不超过 `n/2`，如果超过，就没有约束作用了，相当于 `k = +infinity`。这种情况是之前解决过的。

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if(prices==null||prices.length==0){
            return 0;
        }
        int len=prices.length;
        if(k>len/2){
            int[][] dp=new int[len][2];
            dp[0][0]=0;
            dp[0][1]=-prices[0];
            for(int i=1;i<len;i++){
                dp[i][0]=Math.max(dp[i-1][0],dp[i-1][1]+prices[i]);
                dp[i][1]=Math.max(dp[i-1][1],dp[i-1][0]-prices[i]);
            }
            return dp[len-1][0];
        }else{
            int[][][] dp=new int[len][k+1][2];
            for(int i=0;i<len;i++){
                dp[i][0][0]=0;
                dp[i][0][1]=Integer.MIN_VALUE;
            }
            for(int i=1;i<=k;i++){
                dp[0][i][0]=0;
                dp[0][i][1]=-prices[0];
            }
            for(int i=1;i<len;i++){
                for(int j=1;j<=k;j++){
                    dp[i][j][0]=Math.max(dp[i-1][j][0],dp[i-1][j][1]+prices[i]);
                    dp[i][j][1]=Math.max(dp[i-1][j][1],dp[i-1][j-1][0]-prices[i]);
                }
            }
            return dp[len-1][k][0];
        }
    }
}
```