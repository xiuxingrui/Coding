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
### 解法一
题目说明尚未被充分使用，即 在一个长度为 `n` 的数组 `nums` 里的所有数字都在 `0 ~ n-1` 的范围内 。 此说明含义：数组元素的 索引 和 值 是 一对多 的关系。

因此，可遍历数组并通过交换操作，使元素的 索引 与 值 一一对应（即 `nums[i]=i` ）。因而，就能通过索引映射对应的值，起到与字典等价的作用。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20210116202333.png)

遍历中，第一次遇到数字 `x` 时，将其交换至索引 `x` 处；而当第二次遇到数字 `x` 时，一定有 `nums[x]=x` ，此时即可得到一组重复数字。

算法流程：
- 遍历数组 `nums` ，设索引初始值为 `i=0` :
  - 若 `nums[i]=i`： 说明此数字已在对应索引位置，无需交换，因此跳过；
  - 若 `nums[nums[i]]=nums[i]`： 代表索引 `nums[i]` 处和索引 `i` 处的元素值都为 `nums[i]`，即找到一组重复值，返回此值 `nums[i]`；
  - 否则： 交换索引为 `i` 和 `nums[i]` 的元素值，将此数字交换至对应索引位置。
若遍历完毕尚未返回，则返回 −1 。

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
### 解法二
哈希表:

利用数据结构特点，容易想到使用哈希表（`Set`）记录数组的各个数字，当查找到重复数字则直接返回。

算法流程：

- 初始化： 新建 `HashSet` ，记为 `dic` ；
- 遍历数组 `nums` 中的每个数字 `num` ：
  - 当 `num` 在 `dic` 中，说明重复，直接返回 `num` ；
  - 将 `num` 添加至 `dic` 中；

返回 −1 。本题中一定有重复数字，因此这里返回多少都可以。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> dic = new HashSet<>();
        for(int num : nums) {
            if(dic.contains(num)) return num;
            dic.add(num);
        }
        return -1;
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(N)$： 遍历数组使用 $O(N)$，`HashSet` 添加与查找元素皆为 $O(1)$。
- 空间复杂度 $O(N)$： `HashSet` 占用 $O(N)$ 大小的额外空间。
