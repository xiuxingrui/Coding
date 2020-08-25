# [LeetCode673.最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)
## 题目描述
给定一个未排序的整数数组，找到最长递增子序列的个数。

给定的数组长度不超过 2000 并且结果一定是32位有符号整数。
### 示例
```
输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
```
```
输入: [2,2,2,2,2]
输出: 5
解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5。
```
## 题解
```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int[] dp=new int[nums.length];
        int[] count=new int[nums.length];
        int ans=1;
        for(int i=0;i<nums.length;i++){
            dp[i]=1;
            count[i]=1;
            for(int j=0;j<i;j++){
                if(nums[j]<nums[i]){
                    if(dp[j]+1==dp[i]){
                        count[i]+=count[j];
                    }
                    if(dp[j]+1>dp[i]){
                        dp[i]=dp[j]+1;
                        count[i]=count[j];
                    }
                }
            }
            ans=ans>dp[i]?ans:dp[i];
        }
        int cnt=0;
        for(int i=0;i<nums.length;i++){
            if(dp[i]==ans){
                cnt+=count[i];
            }
        }
        return cnt;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N^2)$。其中 `N` 是 `nums` 的长度。有两个 `for` 循环是 $O(1)$。
- 空间复杂度：$O(N)$，`dp` 和 `count` 所用的空间。