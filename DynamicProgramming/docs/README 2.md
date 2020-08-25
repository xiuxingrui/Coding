# [LeetCode128.最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)
## 题目描述
给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 $O(n)$。

### 示例
```
输入: [100, 4, 200, 1, 3, 2]
输出: 4
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
```
## 题解
### 解法一

### 解法二
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        Arrays.sort(nums);
        int ans=1;
        int count=1;
        for(int i=1;i<nums.length;i++){
            if(nums[i]==nums[i-1]){
                continue;
            }
            if(nums[i]==nums[i-1]+1){
                count++;
                ans=ans>count?ans:count;
            }else{
                count=1;
            }
        }
        return ans;
    }
}
```