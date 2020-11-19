# [LeetCode283.移动零](https://leetcode-cn.com/problems/move-zeroes/)
## 题目描述
给定一个数组 `nums`，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

- 必须在原数组上操作，不能拷贝额外的数组。
- 尽量减少操作次数。

### 示例
```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```
## 题解
### 解法一
一次遍历：

使用双指针，左指针指向当前已经处理好的序列的尾部，右指针指向待处理序列的头部。

右指针不断向右移动，每次右指针指向非零数，则将左右指针对应的数交换，同时左指针右移。

注意到以下性质：

- 左指针左边均为非零数；
- 右指针左边直到左指针处均为零。

因此每次交换，都是将左指针的零与右指针的非零数交换，且非零数的相对顺序并未改变。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int len=nums.length,j=0;
        for(int i=0;i<len;i++){
            if(nums[i]!=0){
                int temp=nums[i];
                nums[i]=nums[j];
                nums[j++]=temp;
            }
        }
        return;
    }
}
```
优化：
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int len=nums.length,j=0;
        for(int i=0;i<len;i++){
            if(nums[i]!=0){
                if(i>j){
                    nums[j]=nums[i];
                    nums[i]=0;
                }
                j++;
            }
        }
        return;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 为序列长度。每个位置至多被遍历两次。
- 空间复杂度：$O(1)$。只需要常数的空间存放若干变量。

### 解法二
需要用两次遍历：
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int len=nums.length;
        int idx=0;
        for(int i=0;i<len;i++){
            if(nums[i]!=0){
                nums[idx++]=nums[i];
            }
        }
        for(int j=idx;j<len;j++){
            nums[j]=0;
        }
        return;
    }
}
```
