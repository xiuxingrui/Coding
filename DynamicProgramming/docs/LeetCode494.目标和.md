# [LeetCode494.目标和](https://leetcode-cn.com/problems/target-sum/)
## 题目描述
给定一个非负整数数组，`a1, a2, ..., an`, 和一个目标数，`S`。现在你有两个符号 `+` 和 `-`。对于数组中的任意一个整数，你都可以从 `+` 或 `-`中选择一个符号添加在前面。

返回可以使最终数组和为目标数 `S` 的所有添加符号的方法数。

### 示例
```
输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5
解释：

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
```

## 题解
### 解法一
原问题是给定一些数字，加加减减，使得它们等于`target`。例如，`1 - 2 + 3 - 4 + 5 = target(3)`。如果我们把加的和减的结合在一起，可以写成

```
(1+3+5)  +  (-2-4) = target(3)
-------     ------
 -> 正数    -> 负数
```

所以，我们可以将原问题转化为： 找到`nums`一个正子集和一个负子集，使得总和等于`target`，统计这种可能性的总数。

```java
sum(P) - sum(N) = target
                  （两边同时加上sum(P)+sum(N)）
sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N)
            (因为 sum(P) + sum(N) = sum(nums))
                       2 * sum(P) = target + sum(nums)

```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200901214925.png)

因此，原来的问题已转化为一个求子集的和问题： 找到`nums`的一个子集 `P`，使得

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200901214629.png)

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        if (nums == null || nums.length == 0) return 0;
        int sums = 0;
        for (int num : nums) sums += num;
        if (sums < S || (S + sums) % 2 == 1) return 0;
        int p = (S + sums) / 2;
        int[] dp = new int[p + 1];
        dp[0] = 1;
        for (int num : nums) {
            for (int i = p; i >= num; i--) {
                dp[i] += dp[i - num];
            }
        }
        return dp[p];
    }
}
```
### 解法二
这道题也是一个常见的背包问题，我们可以用类似求解背包问题的方法来求出可能的方法数。

我们用 `dp[i][j]` 表示用数组中的前 `i` 个元素，组成和为 `j` 的方案数。考虑第 `i` 个数 `nums[i]`，它可以被添加 `+` 或 `-`，因此状态转移方程如下：

```java
dp[i][j] = dp[i - 1][j - nums[i]] + dp[i - 1][j + nums[i]]
```
也可以写成递推的形式：
```java
dp[i][j + nums[i]] += dp[i - 1][j]
dp[i][j - nums[i]] += dp[i - 1][j]
```
由于数组中所有数的和不超过 1000，那么 `j` 的最小值可以达到 -1000。在很多语言中，是不允许数组的下标为负数的，因此我们需要给 `dp[i][j]` 的第二维预先增加一个值。

```java
public class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sumAll=0;
        for(int num:nums){
            sumAll+=num;
        }
        int[][] dp = new int[nums.length][2*sumAll+1];
        dp[0][nums[0] + sumAll] = 1;
        dp[0][-nums[0] + sumAll] += 1;
        for (int i = 1; i < nums.length; i++) {
            for (int sum = -sumAll; sum <= sumAll; sum++) {
                if (dp[i - 1][sum + sumAll] > 0) {//判断条件保证sum是可取的值，不会越界
                    dp[i][sum + nums[i] + sumAll] += dp[i - 1][sum + sumAll];
                    dp[i][sum - nums[i] + sumAll] += dp[i - 1][sum + sumAll];
                }
            }
        }
        return S > sumAll? 0 : dp[nums.length - 1][S + sumAll];
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N∗sum)$，其中 `N` 是数组 `nums` 的长度。
- 空间复杂度：$O(N∗sum)$
### 解法三:DFS
```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        return dfs(nums,S,0);
    }
    public int dfs(int[] nums,int sum,int idx){
        if(sum==0&&idx==nums.length){
            return 1;
        }
        if(idx>nums.length-1){
            return 0;
        }
        int ans=0;
        ans+=dfs(nums,sum+nums[idx],idx+1);
        ans+=dfs(nums,sum-nums[idx],idx+1);
        return ans;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(2^N)$，其中 `N` 是数组 `nums` 的长度。
- 空间复杂度：$O(N)$，为递归使用的栈空间大小。




