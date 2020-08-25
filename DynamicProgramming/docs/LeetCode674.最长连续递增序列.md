# [LeetCode674.最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)
## 题目描述
给定一个未经排序的整数数组，找到最长且连续的的递增序列，并返回该序列的长度。

数组长度不会超过10000。

### 示例
```
输入: [1,3,5,4,7]
输出: 3
解释: 最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为5和7在原数组里被4隔开。 
```
```
输入: [2,2,2,2,2]
输出: 1
解释: 最长连续递增序列是 [2], 长度为1。
```
## 题解
- `count` 为当前元素峰值，`ans`为最大峰值
- 初始化 `count = 1`
- 从 0 位置开始遍历，遍历时根据前后元素状态判断是否递增，递增则 `count++`，递减则 `count=1`
- 如果 `count>ans`，则更新 `ans`
- 直到循环结束
```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int ans=1,count=1;
        for(int i=1;i<nums.length;i++){
            if(nums[i]>nums[i-1]){
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
### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是 `nums` 的长度。我们通过 `nums` 执行一个循环。
- 空间复杂度：$O(1)$
