# [LeetCode27.移除元素](https://leetcode-cn.com/problems/remove-element/)
## 题目描述
给你一个数组 `nums` 和一个值 `val`，你需要 **原地** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 $O(1)$ 额外空间并 **原地** 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

### 示例
```
给定 nums = [3,2,2,3], val = 3,

函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。

你不需要考虑数组中超出新长度后面的元素。
```
```
给定 nums = [0,1,2,2,3,0,4,2], val = 2,

函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。
```
## 题解
### 解法一
自己的写法
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        if(nums.length==0){
            return 0;
        }
        int i=0,j=nums.length-1;
        while(i<j){
            while(i<j&&nums[i]!=val){
                i++;
            }
            while(i<j&&nums[j]==val){
                j--;
            }
            if(i<j){
                nums[i]=nums[j];
                nums[j]=val;
            }
        }
        if(nums[i]==val){
            return i;
        }else{
            return i+1;//比如输入是[2] 3
        }
    }
}
```
### 解法二
```java
public int removeElement(int[] nums, int val) {
    int i = 0;
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != val) {
            nums[i] = nums[j];
            i++;
        }
    }
    return i;
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，假设数组总共有 `n` 个元素，`i` 和 `j` 至少遍历 `2n` 步。
- 空间复杂度：$O(1)$。
  
### 解法三:要删除的元素很少时
```java
public int removeElement(int[] nums, int val) {
    int i = 0;
    int n = nums.length;
    while (i < n) {
        if (nums[i] == val) {
            nums[i] = nums[n - 1];
            // reduce array size by one
            n--;
        } else {
            i++;
        }
    }
    return n;
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，`i` 和 `n` 最多遍历 `n` 步。在这个方法中，赋值操作的次数等于要删除的元素的数量。因此，如果要移除的元素很少，效率会更高。
- 空间复杂度：$O(1)$。
