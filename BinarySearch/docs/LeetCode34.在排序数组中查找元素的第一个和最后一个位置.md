# [LeetCode34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
## 题目描述
给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 $O(logn)$ 级别。

如果数组中不存在目标值，返回 `[-1, -1]`。

### 示例
```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]

输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```
## 题解
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = {-1, -1};
        if(nums.length==0){
            return res;
        }
        int len=nums.length;
        int left=0,right=len;
        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]>=target){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        if(left==len||nums[left]!=target){
            return res;
        }
        res[0]=left;
        left=0;
        right=len;
        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]>target){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        res[1]=left-1;
        return res;
    }
}
```
### 复杂度分析
- 时间复杂度： $O\left(\log _{2} n\right)$：由于二分查找每次将搜索区间大约划分为两等分，所以至多有$\left\lceil\log _{2} n\right\rceil$次迭代。二分查找的过程被调用了两次，所以总的时间复杂度是对数级别的。

- 空间复杂度：$O(1)$:所有工作都是原地进行的，所以总的内存空间是常数级别的。
