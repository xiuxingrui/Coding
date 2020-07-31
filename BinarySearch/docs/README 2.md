# [面试题08.03.魔术索引](https://leetcode-cn.com/problems/magic-index-lcci/)
## 题目描述
魔术索引。 在数组`A[0...n-1]`中有所谓的魔术索引，满足条件`A[i] = i`。给定一个递增有序整数数组，编写一种方法找出魔术索引，若有的话，在数组`A`中找出一个魔术索引，如果没有，则返回-1。若有多个魔术索引，返回索引值最小的一个。

`nums`长度在`[1, 1000000]`之间
### 示例
``` 输入：nums = [0, 2, 3, 4, 5]
 输出：0
 说明: 0下标的元素为0
```
```
输入：nums = [1, 1, 1]
输出：1
```
## 题解
### 解法一:
```java
class Solution {
    public int findMagicIndex(int[] nums) {
        return getAnswer(nums, 0, nums.length - 1);
    }

    public int getAnswer(int[] nums, int left, int right) {
        if (left > right) {
            return -1;
        }
        int mid = (right - left) / 2 + left;
        int leftAnswer = getAnswer(nums, left, mid - 1);
        if (leftAnswer != -1) {
            return leftAnswer;
        } else if (nums[mid] == mid) {
            return mid;
        }
        return getAnswer(nums, mid + 1, right);
    }
}
```
#### 复杂度分析
- 时间复杂度：最坏情况下会达到 $O(n)$ 的时间复杂度，其中 `n` 为数组的长度。
- 空间复杂度：递归函数的空间取决于调用的栈深度，而最坏情况下我们会递归 `n` 层，即栈深度为 $O(n)$，因此空间复杂度最坏情况下为 $O(n)$。
### 解法二:
```java
class Solution {
    public int findMagicIndex(int[] nums) {
        int i=0;
        while(i<nums.length){
            if(nums[i]==i){
                return nums[i];
            }else if(nums[i]>i){
                i=nums[i];
            }else{
                i++;
            }
        }
        return -1;
    }
}
```

