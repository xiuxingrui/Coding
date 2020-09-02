# [LeetCode746.使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)
## 题目描述
- 数组的每个索引作为一个阶梯，第 `i`个阶梯对应着一个非负数的体力花费值 `cost[i]`(索引从0开始)。
- 每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。
- 您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

- `cost` 的长度将会在 `[2, 1000]`。
- 每一个 `cost[i]` 将会是一个`Integer`类型，范围为 `[0, 999]`。

### 示例
```
输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
```
```
输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。
```
## 题解
### 解法一
自己的解法:
```java
class Solution {
    //达到楼层顶部的最低花费，实际上是恰好到达顶部的再上一级的最小花费，毕竟到达顶部存在从下一级跨两步越过顶部这一情况，设cost[len]=0
    public int minCostClimbingStairs(int[] cost) {
        int len=cost.length;
        int[] dp=new int[len+1];
        dp[0]=cost[0];
        dp[1]=cost[1];
        for(int i=2;i<len;i++){
            dp[i]=Math.min(dp[i-1],dp[i-2])+cost[i];
        }
        dp[len]=Math.min(dp[len-1],dp[len-2]);
        return dp[len];
    }
}
```
