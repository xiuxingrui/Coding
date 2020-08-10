# [LeetCode81.搜索旋转排序数组II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)
## 题目描述
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,0,1,2,2,5,6]` 可能变为 `[2,5,6,0,0,1,2]` )。

编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 `true`，否则返回 `false`。

进阶:

- 这是 搜索旋转排序数组 的延伸题目，本题中的 `nums`  可能包含重复元素。
- 这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？
### 示例
```
输入: nums = [2,5,6,0,0,1,2], target = 0
输出: true
```
```
输入: nums = [2,5,6,0,0,1,2], target = 3
输出: false
```
## 题解
1. 直接`nums[mid] == target`
2. 当数组为 `[1,2,1,1,1]`,`nums[mid] == nums[left] == nums[right]`，需要 `left++`, `right --`;
3. 当 `nums[left]<= nums[mid]`，说明是在左半边的递增区域
   1. `nums[left] <=target < nums[mid]`，说明 `target` 在 `left` 和 `mid` 之间。我们令 `right = mid - 1`;
   2. 不在之间，我们令 `left = mid + 1`;
4. 当 `nums[mid] < nums[right]`，说明是在右半边的递增区域
   1. `nums[mid] < target <= nums[right]`，说明 `target` 在 `mid` 和 `right` 之间，我们令 `left = mid + 1`
   2. 不在之间，我们令 `right = mid - 1`;
```java
class Solution {
    public boolean search(int[] nums, int target) {
        int len=nums.length;
        int left=0,right=len-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target){
                return true;
            }
            if(nums[left]==nums[mid]&&nums[mid]==nums[right]){
                left++;
                right--;
                continue;
            }
            if(nums[left]<=nums[mid]){
                if(target>=nums[left]&&target<nums[mid]){
                    right=mid-1;
                }else{
                    left=mid+1;
                }
            }else{
                if(target>nums[mid]&&target<=nums[right]){
                    left=mid+1;
                }else{
                    right=mid-1;
                }
            }
        }
        return false;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(logn)$。