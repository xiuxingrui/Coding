# [LeetCode259.较小的三数之和](https://leetcode-cn.com/problems/3sum-smaller/)
## 题目描述
给定一个长度为 `n` 的整数数组和一个目标值 `target`，寻找能够使条件 `nums[i] + nums[j] + nums[k] < target` 成立的三元组  `i`, `j`, `k` 个数（`0 <= i < j < k < n`）。

### 示例：
```
输入: nums = [-2,0,1,3], target = 2
输出: 2 
解释: 因为一共有两个三元组满足累加和小于 2:
     [-2,0,1]
     [-2,0,3]
```
## 题解
```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int count=0;
        for(int i=0;i<nums.length-2;i++){
            int j=i+1,k=nums.length-1;
            while(j<k){
                if(nums[i]+nums[j]+nums[k]<target){
                    count+=k-j;
                    j++;
                }else{
                    k--;
                }
            }
        }
        return count;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n^2)$。在每一步中，要么 `left` 向右移动一位，要么 `right` 向左移动一位。当 `left≥right` 时循环结束，因此它的时间复杂度为 $O(n)$。加上外层的循环，总的时间复杂度为 $O(n^2)$。
- 空间复杂度：$O(1)$。
