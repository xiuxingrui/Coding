# [LeetCode215.数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)
## 题目描述
在未排序的数组中找到第 `k` 个最大的元素。请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。


你可以假设 `k` 总是有效的，且 `1≤k≤数组的长度`。
### 示例
```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```
```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```
## 题解
### 解法二:
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int len=nums.length;
        return search(nums,0,nums.length-1,len-k);
    }
    public int search(int[] nums,int start,int end,int index){
        int ret=partition(nums,start,end);
        if(ret==index){
            return nums[index];
        }else if(ret>index){
            return search(nums,start,ret-1,index);
        }else{
            return search(nums,start+1,end,index);
        }
    }
    public int partition(int[] nums,int start,int end){
        int pivot=nums[start];
        while(start<end){
            while(start<end&&nums[end]>pivot){
                end--;
            }
            nums[start]=nums[end];
            while(start<end&&nums[start]<=pivot){
                start++;
            }
            nums[end]=nums[start];
        }
        nums[start]=pivot;
        return start;
    }
}
```
