# [LeetCode53.最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
## 题目描述
给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

### 示例
```
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```
## 题解
「子数组」、「子序列」问题的状态设计的特点是：以 `nums[i]` 结尾，这是一个经验，可以简化讨论。

状态定义： 设动态规划列表 `dp` ，`dp[i]` 代表以元素 `nums[i]` 为结尾的连续子数组最大和。

为何定义最大和 `dp[i]` 中必须包含元素 `nums[i]`：保证 `dp[i]` 递推到 `dp[i+1]` 的正确性；如果不包含 `nums[i]` ，递推时则不满足题目的 连续子数组 要求.

转移方程： 若 `dp[i−1]≤0` ，说明 `dp[i−1]` 对 `dp[i]` 产生负贡献，即 `dp[i−1]+nums[i]` 还不如 `nums[i]` 本身大。

其实可以理解为，以`nums[i]`结尾的子数组和，有两种情况，包括`nums[i]`和只有`nums[i]`一个元素这两种情况，而不仅仅包括`nums[i]`的情况，其最优解为`dp[i-1]+nums[i]`。

- 当 `dp[i−1]>0` 时：执行 `dp[i]=dp[i−1]+nums[i]`；
- 当 `dp[i−1]≤0` 时：执行 `dp[i]=nums[i]`；

初始状态： `dp[0]=nums[0]`，即以 `nums[0]`结尾的连续子数组最大和为 `nums[0]`。

返回值： 返回 `dp` 列表中的最大值，代表全局最大值。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans=nums[0];
        int[] dp=new int[nums.length];
        dp[0]=nums[0];
        for(int i=1;i<nums.length;i++){
            dp[i]=Math.max(dp[i-1]+nums[i],nums[i]);
            ans=ans>dp[i]?ans:dp[i];
        }
        return ans;
    }
}
```
由于 `dp[i]` 只与 `dp[i−1]` 和 `nums[i]` 有关系，因此可以将原数组 `nums` 用作 `dp` 列表，即直接在 `nums`上修改即可。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans=nums[0];
        for(int i=1;i<nums.length;i++){
            nums[i]=Math.max(nums[i-1]+nums[i],nums[i]);
            ans=ans>nums[i]?ans:nums[i];
        }
        return ans;
    }
}
```
### 复杂度分析：

- 时间复杂度 $O(N)$： 线性遍历数组 `nums` 即可获得结果，使用 $O(N)$ 时间。
- 空间复杂度 $O(1)$： 使用常数大小的额外空间。
