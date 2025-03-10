# [LeetCode123.买卖股票的最佳时机III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)
## 题目描述
给定一个数组，它的第 `i` 个元素是一支给定的股票在第 `i` 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

### 示例
```
输入: [3,3,5,0,0,3,1,4]
输出: 6
解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```
```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。   
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```
```
输入: [7,6,4,3,1] 
输出: 0 
解释: 在这个情况下, 没有交易完成, 所以最大利润为 0。
```
## 题解
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null||prices.length==0){
            return 0;
        }
        int len=prices.length;
        int[][][] dp=new int[len][3][2];
        for(int i=0;i<len;i++){
            dp[i][0][0]=0;
            dp[i][0][1]=Integer.MIN_VALUE;
        }
        for(int i=1;i<3;i++){
            dp[0][i][0]=0;
            dp[0][i][1]=-prices[0];
        }
        for(int i=1;i<len;i++){
            for(int j=1;j<3;j++){
                dp[i][j][0]=Math.max(dp[i-1][j][0],dp[i-1][j][1]+prices[i]);
                dp[i][j][1]=Math.max(dp[i-1][j][1],dp[i-1][j-1][0]-prices[i]);
            }
        }
        return dp[len-1][2][0];
    }
}
```
状态压缩:
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null||prices.length==0){
            return 0;
        }
        int len=prices.length;
        int[][] dp=new int[3][2];
        dp[0][0]=0;
        dp[0][1]=Integer.MIN_VALUE;
        for(int i=1;i<3;i++){
            dp[i][0]=0;
            dp[i][1]=-prices[0];
        }
        for(int i=1;i<len;i++){
            for(int j=1;j<3;j++){
                dp[j][0]=Math.max(dp[j][0],dp[j][1]+prices[i]);
                dp[j][1]=Math.max(dp[j][1],dp[j-1][0]-prices[i]);
            }
        }
        return dp[2][0];
    }
}
```