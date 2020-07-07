# [LeetCode189.旋转数组](https://leetcode-cn.com/problems/rotate-array/)
## 题目描述
给定一个数组，将数组中的元素向右移动 `k` 个位置，其中 `k` 是非负数。

### 示例
```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]

输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]

```
### 说明
- 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
- 要求使用空间复杂度为 $O(1)$ 的 **原地** 算法。
  
## 题集
### 解法一
要注意是**右移**还是**左移**，如果是**左移**，则`reverse(nums, 0, k - 1);`在第一句。
```java
class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, nums.length-k, nums.length - 1);
        reverse(nums, 0, nums.length-k-1);
        reverse(nums, 0, nums.length - 1);
    }
    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(n)$ 。 `n` 个元素被反转了总共 3 次。
- 空间复杂度：$O(1)$ 。 没有使用额外的空间。
### 解法二
环状替代:

如果我们直接把每一个数字放到它最后的位置，但这样的后果是遗失原来的元素。因此，我们需要把被替换的数字保存在变量 `temp` 里面。然后，我们将被替换数字(`temp`)放到它正确的位置，并继续这个过程 `n` 次， `n` 是数组的长度。这是因为我们需要将数组里所有的元素都移动。

但当`k`和`n`的最大公约数不为1时,我们会发现在没有遍历所有数字的情况下回到出发数字。此时，我们应该从下一个数字开始再重复相同的过程。
```java
public class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        int count = 0;
        for (int start = 0; count < nums.length; start++) {
            int current = start;
            int prev = nums[start];
            do {
                int next = (current + k) % nums.length;
                int temp = nums[next];
                nums[next] = prev;
                prev = temp;
                current = next;
                count++;
            } while (start != current);
        }
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(n∗k)$。每个元素都被移动 1 步($O(n)$) `k`次($O(k)$) 。
- 空间复杂度：$O(1)$。没有额外空间被使用。
### 解法三
```java
class Solution {
    public void rotate(int[] nums, int k) {
        k%=nums.length;
        for(int i=0;i<k;++i){
            int temp=nums[nums.length-1];
            for(int j=nums.length-1;j>0;j--){
                nums[j]=nums[j-1];
            }
            nums[0]=temp;
        }
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(n∗k)$。每个元素都被移动 1 步($O(n)$) `k`次($O(k)$) 。
- 空间复杂度：$O(1)$。没有额外空间被使用。

