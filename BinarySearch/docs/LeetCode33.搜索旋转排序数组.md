# [LeetCode33.搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
## 题目描述
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 $O(logn)$ 级别。

### 示例
```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```
```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```
## 题解
### 解法一:
```java
public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        int start = 0;
        int end = nums.length - 1;
        int mid;
        while (start <= end) {
            mid = start + (end - start) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            //前半部分有序,注意此处用小于等于,如果没有=号，用例为[3,1],1时会出错。
            if (nums[start] <= nums[mid]) {
                //target在前半部分
                if (target >= nums[start] && target < nums[mid]) {
                    end = mid - 1;
                } else {
                    start = mid + 1;
                }
            } else {
                if (target <= nums[end] && target > nums[mid]) {
                    start = mid + 1;
                } else {
                    end = mid - 1;
                }
            }

        }
        return -1;

    }
```
写法二:
```java
public int search(int[] nums, int target) {
        if(nums == null || nums.length == 0){
            return -1;
        }
        int start = 0;
        int end = nums.length - 1;

        while (start <= end){
            int mid = start + (end -start)/2;
            if (nums[mid] == target){
                return mid;
            }

            //后半部分有序
            if(nums[mid] < nums[end]){
                if(nums[mid] < target && target <= nums[end]){
                    start = mid + 1;
                } else {
                    end = mid - 1;
                }

            } else {
                 if(nums[mid] > target && target >= nums[start]){
                    end = mid - 1;
                    
                } else {
                    start = mid + 1;
                    
                }


            }
        }
        return -1;
        
    }
```
### 自己解法
```java
class Solution {
    public int search(int[] nums, int target) {
        return binarySearch(nums,0,nums.length-1,target);
    }
    public int binarySearch(int[] nums,int left,int right,int target){
        if(left>right){
            return -1;
        }
        int mid=left+(right-left)/2;
        if(nums[mid]==target){
            return mid;
        }else if(mid==left){
            return binarySearch(nums,mid+1,right,target);
        }else if(mid==right){
            return binarySearch(nums,left,mid-1,target);
        }else{
            if(nums[left]<=nums[mid-1]){
                if(target>=nums[left]&&target<=nums[mid-1]){
                    return binarySearch(nums,left,mid-1,target);
                }else{
                    return binarySearch(nums,mid+1,right,target);
                }
            }else{
                if(target>=nums[mid+1]&&target<=nums[right]){
                    return binarySearch(nums,mid+1,right,target);
                }else{
                    return binarySearch(nums,left,mid-1,target);
                }
            }
        }
    }
}
```