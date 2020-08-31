# [LintCode563.背包问题V](https://www.lintcode.com/problem/backpack-v/description)
## 题目描述
给出 `n` 个物品, 以及一个数组, `nums[i]` 代表第`i`个物品的大小, 保证大小均为正数, 正整数 `target` 表示背包的大小, 找到能填满背包的方案数。

每一个物品只能使用一次
## 题解
```java
public class Solution {
    /**
     * @param nums: an integer array and all positive numbers
     * @param target: An integer
     * @return: An integer
     */
    public int backPackV(int[] nums, int target) {
        // write your code here
        int[] dp=new int[target+1];
        dp[0]=1;
        for(int i=0;i<nums.length;i++){
            for(int j=target;j>=nums[i];j--){
                dp[j]+=dp[j-nums[i]];
            }
        }
        return dp[target];
    }
}
```