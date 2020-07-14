# (LeeetCode35.搜索插入位置)(https://leetcode-cn.com/problems/search-insert-position/)
## 题目描述
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

### 示例
```
输入: [1,3,5,6], 5
输出: 2

输入: [1,3,5,6], 2
输出: 1

输入: [1,3,5,6], 7
输出: 4

输入: [1,3,5,6], 0
输出: 0
```
## 题解
### 解法一
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left=0,right=nums.length-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target){
                return mid;
            }else if(nums[mid]>target){
                right=mid-1;
            }else{
                left=mid+1;
            }
        }
        return left;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(logN)$，这里 `N` 是数组的长度，每一次都将问题的规模缩减为原来的一半，因此时间复杂度是对数级别的；
- 空间复杂度：$O(1)$。

### 解法二
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left=0,right=nums.length;
        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]>=target){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        return left;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(logN)$，这里 `N` 是数组的长度，每一次都将问题的规模缩减为原来的一半，因此时间复杂度是对数级别的；
- 空间复杂度：$O(1)$。
