# [LeetCode167.两数之和II-输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)
## 题目描述
给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 `index1` 和 `index2`，其中 `index1` 必须小于 `index2`。

- 返回的下标值（`index1` 和 `index2`）不是从零开始的。
- 你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

### 示例
```
输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```
### 题解
## 解法一
初始时两个指针分别指向第一个元素位置和最后一个元素的位置。每次计算两个指针指向的两个元素之和，并和目标值比较。如果两个元素之和等于目标值，则发现了唯一解。如果两个元素之和小于目标值，则将左侧指针右移一位。如果两个元素之和大于目标值，则将右侧指针左移一位。移动指针之后，重复上述操作，直到找到答案。

使用双指针的实质是缩小查找范围。那么会不会把可能的解过滤掉？答案是不会。假设 `numbers[i]+numbers[j]=target` 是唯一解，其中 `0≤i<j≤numbers.length−1`。初始时两个指针分别指向下标 0 和下标 `numbers.length−1`，左指针指向的下标小于或等于 `i`，右指针指向的下标大于或等于 `j`。除非初始时左指针和右指针已经位于下标 `i` 和 `j`，否则一定是左指针先到达下标 `i` 的位置或者右指针先到达下标 `j` 的位置。

如果左指针先到达下标 `i` 的位置，此时右指针还在下标 `j` 的右侧，`sum>target`，因此一定是右指针左移，左指针不可能移到 `i` 的右侧。

如果右指针先到达下标 `j` 的位置，此时左指针还在下标 `i` 的左侧，`sum<target`，因此一定是左指针右移，右指针不可能移到 `j` 的左侧。

由此可见，在整个移动过程中，左指针不可能移到 `i` 的右侧，右指针不可能移到 `j` 的左侧，因此不会把可能的解过滤掉。由于题目确保有唯一的答案，因此使用双指针一定可以找到答案。
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int low = 0, high = numbers.length - 1;
        while (low < high) {
            int sum = numbers[low] + numbers[high];
            if (sum == target) {
                return new int[]{low + 1, high + 1};
            } else if (sum < target) {
                ++low;
            } else {
                --high;
            }
        }
        return new int[]{-1, -1};
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是数组的长度。两个指针移动的总次数最多为 `n` 次。
- 空间复杂度：$O(1)$。
### 解法二
在数组中找到两个数，使得它们的和等于目标值，可以首先固定第一个数，然后寻找第二个数，第二个数等于目标值减去第一个数的差。利用数组的有序性质，可以通过二分查找的方法寻找第二个数。为了避免重复寻找，在寻找第二个数时，只在第一个数的右侧寻找。

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] res=new int[2];
        for(int i=0;i<numbers.length-1;i++){
            int temp=binarySearch(numbers,target-numbers[i],i+1,numbers.length-1);
            if(temp!=-1){
                res[0]=i+1;
                res[1]=temp+1;
                break;
            }
        }
        return res;
    }
    public int binarySearch(int[] nums,int num,int left,int right){
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==num){
                return mid;
            }else if(nums[mid]>num){
                right=mid-1;
            }else{
                left=mid+1;
            }
        }
        return -1;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogn)$，其中 `n` 是数组的长度。需要遍历数组一次确定第一个数，时间复杂度是 $O(n)$，寻找第二个数使用二分查找时间复杂度是 $O(logn)$，因此总时间复杂度是 $O(nlogn)$。
- 空间复杂度：$O(1)$。