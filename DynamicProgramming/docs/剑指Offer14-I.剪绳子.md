# [LeetCode剑指Offer14-I.剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)
## 题目描述
给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（`m`、`n`都是整数，`n>1`并且`m>1`），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]`可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

### 示例
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```
```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```
## 题解
```java
class Solution {
    public int cuttingRope(int n) {
        int[] dp=new int[n+1];
        for(int i=2;i<=n;i++){
            for(int j=1;j<i;j++){
                dp[i]=Math.max(dp[i],Math.max(j*(i-j),j*dp[i-j]));
            }
        }
        return dp[n];
    }
}
```
