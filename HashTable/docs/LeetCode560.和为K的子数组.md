# [LeetCode560.和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)
## 题目描述
给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。
### 示例
```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```
### 说明
- 数组的长度为 `[1, 20,000]`。
- 数组中元素的范围是 `[-1000, 1000]` ，且整数 `k` 的范围是 `[-1e7, 1e7]`。

## 题解
### 解法一
```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, pre = 0;
        HashMap < Integer, Integer > mp = new HashMap < > ();
        mp.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            pre += nums[i];
            if (mp.containsKey(pre - k))
                count += mp.get(pre - k);
            mp.put(pre, mp.getOrDefault(pre, 0) + 1);
        }
        return count;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 为数组的长度。我们遍历数组的时间复杂度为 $O(n)$，中间利用哈希表查询删除的复杂度均为 $O(1)$，因此总时间复杂度为 $O(n)$。
- 空间复杂度：$O(n)$，其中 `n` 为数组的长度。哈希表在最坏情况下可能有 `n` 个不同的键值，因此需要 $O(n)$ 的空间复杂度。
### 解法二
```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int start = 0; start < nums.length; ++start) {
            int sum = 0;
            for (int end = start; end >= 0; --end) {
                sum += nums[end];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n^2)$，其中 `n` 为数组的长度。枚举子数组开头和结尾需要 $O(n^2)$的时间，其中求和需要 $O(1)$ 的时间复杂度，因此总时间复杂度为$(n^2)$。

- 空间复杂度：$O(1)$。只需要常数空间存放若干变量。

