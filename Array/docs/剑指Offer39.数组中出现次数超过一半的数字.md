# [剑指Offer39.数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)
## 题目描述
数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。
### 示例1
```
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```
### 示例2
```
输入: [2,2,1,1,1,2,2]
输出: 2
```
### 限制
```
1 <= 数组长度 <= 50000
```
## 题解
### 解法一:摩尔计数法
```java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0;
        for(int num : nums){
            if(votes == 0) x = num;
            votes += num == x ? 1 : -1;
        }
        return x;
    }
}
```
#### 复杂度分析
时间复杂度：$O(n)$。只对数组进行了一次遍历。

空间复杂度：$O(1)$。算法只需要常数级别的额外空间。

#### 题目拓展
- 由于题目明确给定 **给定的数组总是存在多数元素** ，因此本题不用考虑 **数组中不存在众数** 的情况。
- 若考虑，则需要加入一个 “验证环节” ，遍历数组 `nums` 统计 `xx` 的数量。
  - 若 `x` 的数量超过数组长度一半，则返回 `x` ；
  - 否则，返回 0 （这里根据不同题目的要求而定）。

- 时间复杂度仍为 $O(N)$，空间复杂度仍为 $O(1)$ 。
```java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0, count = 0;
        for(int num : nums){
            if(votes == 0) x = num;
            votes += num == x ? 1 : -1;
        }
        // 验证 x 是否为众数
        for(int num : nums)
            if(num == x) count++;
        return count > nums.length / 2 ? x : 0; // 当无众数时返回 0
    }
}
```