# [LeetCode377.组合总和Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/)
## 题目描述
给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。
### 示例
```
nums = [1, 2, 3]
target = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 7。
```
## 题解
**此种类型的题不能用二维DP**，照搬dp[i][j]的定义形式是错误的！！！

- 输入数组的每个元素可以使用多次，这一点和「完全背包」问题有点像；
- 顺序不同的序列被视作不同的组合，这一点和所有的「背包问题」都不同，与 518. 零钱兑换 II 问题不同的地方就在这一点。

- 遇到这一类问题，做一件事情有很多种做法，每一种做法有若干个步骤，脑子里能想到的常规思路大概有「回溯搜索」、「动态规划」；
- 由于不用得到具体的组合表示，因此考虑使用「动态规划」来解。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200912011921.png)

很容易发现「重复问题」，因此，我们可以使用「动态规划」来做，如果题目问具体的解，那么用「回溯搜索」做（「力扣」第 39 题：组合之和）。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200912011941.png)

“动态规划”的两个步骤是思考“状态”以及“状态转移方程”。

1. 状态

对于“状态”，我们首先思考能不能就用问题当中问的方式定义状态，上面递归树都画出来了。当然就用问题问的方式。

`dp[i]` ：对于给定的由正整数组成且不存在重复数字的数组，和为 `i` 的组合的个数。

思考输出什么？因为状态就是问题当中问的方式而定义的，因此输出就是最后一个状态 `dp[n]`。

2.状态转移方程

由上面的树形图，可以很容易地写出状态转移方程：

```
dp[i] = sum{dp[i - num] for num in nums and if i >= num}
```
注意：在 0 这一点，我们定义 `dp[0] = 1` 的，它表示如果 `nums` 里有一个数恰好等于 `target`，它单独成为 1 种可能。




```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        if(nums==null||nums.length==0){
            return 0;
        }
        if(target==0){
            return 1;
        }
        int[] dp=new int[target+1];
        dp[0]=1;
        for(int i=1;i<=target;i++){
            for(int j=0;j<nums.length;j++){
                if(i>=nums[j]){
                    dp[i]+=dp[i-nums[j]];
                }
            }
        }
        return dp[target];
    }
}
```
