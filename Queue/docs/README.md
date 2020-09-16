# [LeetCode239.滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
## 题目描述
给定一个数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

你能在线性时间复杂度内解决此题吗？

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

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