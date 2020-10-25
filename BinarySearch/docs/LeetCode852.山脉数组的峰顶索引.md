# [LeetCode852.山脉数组的峰顶索引](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/)
## 题解
我们把符合下列属性的数组 `A` 称作山脉：

- `A.length >= 3`
- 存在 `0 < i < A.length - 1` 使得`A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`
给定一个确定为山脉的数组，返回任何满足 `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]` 的 `i` 的值。

- `3 <= A.length <= 10000`
- `0 <= A[i] <= 10^6`
- `A` 是如上定义的山脉
### 示例
```
输入：[0,1,0]
输出：1
```
```
输入：[0,2,1,0]
输出：1
```
## 题解
### 解法一
二分法：

模板1：`while(left<=right)`

循环体内有3个分支,在循环体中返回目标索引

```java
class Solution {
    public int peakIndexInMountainArray(int[] nums) {
        int left=0;
        int right=nums.length-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]>nums[mid+1]&&nums[mid]>nums[mid-1]){
                return mid;
            }
            else if(nums[mid]>nums[mid+1]){
                right=mid-1;
            }
            else {
                left=mid+1;
            }
        }
        // 这里return什么都可以，因为对于此题来说，在循环体内一定会返回
        return -1;

    }
}
```
模板2：`while(left<right)`
循环体内有2个分支,在循环体外返回目标索引，在循环体内缩减搜索区间
```java
class Solution {
    public int peakIndexInMountainArray(int[] nums) {
        int left=0;
        int right=nums.length-1;
        while(left<right){
            int mid=left+(right-left)/2;
            // 缩减区间为[mid+1,right]
            if(nums[mid]<nums[mid+1]){
                left=mid+1;
            }
            // 缩减区间为[left,mid]
            else {
                right=mid;
            }
        }
        // left=right时退出循环，返回left或right是一样的
        return left;

    }
}
```
#### 复杂度分析
- 时间复杂度：$O(logN)$，其中 `N` 是 `A` 的长度。
- 空间复杂度：$O(1)$。
### 解法二
从左往右扫描直到山的高度不再增长为止，停止增长点就是峰顶。

```java
class Solution {
    public int peakIndexInMountainArray(int[] A) {
        int i = 0;
        while (A[i] < A[i+1]) i++;
        return i;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是 `A` 的长度。
- 空间复杂度：$O(1)$。



