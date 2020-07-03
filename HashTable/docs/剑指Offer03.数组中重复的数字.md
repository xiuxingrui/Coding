# [剑指Offer03.数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)
## 题目描述
找出数组中重复的数字。

在一个长度为 `n` 的数组 `nums` 里的所有数字都在 `0～n-1` 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

### 示例
```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```
### 限制
`2 <= n <= 100000`
## 题解
### 解法一:
```java
public class Solution {

    public int findRepeatNumber(int[] nums) {
        int len = nums.length;

        for (int i = 0; i < len; i++) {
            // 如果当前的数 nums[i] 没有在下标为 i 的位置上，就把它交换到下标 i 上
            // 交换过来的数还得做相同的操作，因此这里使用 while
            // 可以在此处将数组输出打印，观察程序运行流程
            // System.out.println(Arrays.toString(nums));

            while (nums[i] != i) {

                if (nums[i] == nums[nums[i]]) {
                    // 如果下标为 nums[i] 的数值 nums[nums[i]] 它们二者相等
                    // 正好找到了重复的元素，将它返回
                    return nums[i];
                }
                swap(nums, i, nums[i]);
            }
        }
        return -1;
    }

    private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
    }
}
```
#### 复杂度分析
- 时间复杂度：`O(N)`，这里 `N` 是数组的长度。虽然 `for` 循环里面套了 `while`，但是每一个数来到它应该在的位置以后，位置就不会再变化。这里用到的是均摊复杂度分析的方法：如果在某一个位置 `while` 循环体执行的次数较多，那么一定在后面的几个位置，根本不会执行 `while` 循环体内的代码，也就是说最坏的情况不会一直出现。也就是说最坏复杂度的情况不会一直出现。
- 空间复杂度：`O(1)`。


