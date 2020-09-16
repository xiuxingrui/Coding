# [剑指Offer59-I.滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)
## 题目描述
给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。


你可以假设 `k` 总是有效的，在输入数组不为空的情况下，`1 ≤ k ≤ 输入数组的大小`。
### 示例
```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
## 题解
### 解法二
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums==null||nums.length==0||k==0){
            return new int[0];
        }
        int len=nums.length;
        int[] ans=new int[len-k+1];
        int idx=0;
        for(int i=0;i<=len-k;i++){
            int maxNum=nums[i];
            for(int j=1;j<k;j++){
                maxNum=maxNum>nums[i+j]?maxNum:nums[i+j];
            }
            ans[idx++]=maxNum;
        }
        return ans;
    }
}
```