# [LintCode562.背包问题IV](https://www.lintcode.com/problem/backpack-iv/description)
## 题目描述
给出 `n` 个物品, 以及一个数组, `nums[i]`代表第`i`个物品的大小, 保证大小均为正数并且没有重复, 正整数 `target` 表示背包的大小, 找到能填满背包的方案数。

每一个物品可以使用无数次

## 题解
```java
public class Solution {
    /**
     * @param nums: an integer array and all positive numbers, no duplicates
     * @param target: An integer
     * @return: An integer
     */
    public int backPackIV(int[] nums, int target) {
        // write your code here
        int[] dp=new int[target+1];
        dp[0]=1;
        for(int i=0;i<nums.length;i++){
            for(int j=nums[i];j<=target;j++){
                dp[j]+=dp[j-nums[i]];
            }
        }
        return dp[target];
    }
}
```
