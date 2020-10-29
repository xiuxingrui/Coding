# [LeetCode45.跳跃游戏II](https://leetcode-cn.com/problems/jump-game-ii/)
## 题目描述
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

假设你总是可以到达数组的最后一个位置。
### 示例
```
输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```
## 题解
### 解法一

### 解法二
`DP`:超时
```java
class Solution {
    public int jump(int[] nums) {
        int len=nums.length;
        int[] dp=new int[len];
        for(int i=1;i<len;i++){
            dp[i]=len;
            for(int j=0;j<i;j++){
                if(nums[j]>=i-j){
                    dp[i]=Math.min(dp[i],dp[j]+1);
                }
            }
        }
        return dp[len-1];
    }
}
```