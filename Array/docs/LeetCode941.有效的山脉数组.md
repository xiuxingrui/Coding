# [LeetCode941.有效的山脉数组](https://leetcode-cn.com/problems/valid-mountain-array/)
## 题目描述
给定一个整数数组 A，如果它是有效的山脉数组就返回 true，否则返回 false。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：

- `A.length >= 3`
- 在 `0 < i < A.length - 1` 条件下，存在 `i` 使得：
  - `A[0] < A[1] < ... A[i-1] < A[i]`
  - `A[i] > A[i+1] > ... > A[A.length - 1]`

- `0 <= A.length <= 10000`
- `0 <= A[i] <= 10000 `

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201025141923.png)
### 示例
```
输入：[2,1]
输出：false
```
```
输入：[3,5,5]
输出：false
```
```
输入：[0,3,2,1]
输出：true
```
## 题解
我们从数组的最左侧开始扫描，直到找到第一个不满足 `A[i] < A[i + 1]` 的 `i`，那么 `i` 就是这个数组的最高点。如果 `i = 0` 或者不存在这样的 `i`（即整个数组都是单调递增的），那么就返回 `false`。否则从 `i` 开始继续扫描，判断接下来的的位置 `j` 是否都满足 `A[j] > A[j + 1]`，若都满足就返回 `true`，否则返回 `false`。

```java
class Solution {
    public boolean validMountainArray(int[] A) {
        int N = A.length;
        int i = 0;

        // walk up
        while (i+1 < N && A[i] < A[i+1])
            i++;

        // peak can't be first or last
        if (i == 0 || i == N-1)
            return false;

        // walk down
        while (i+1 < N && A[i] > A[i+1])
            i++;

        return i == N-1;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是数组 `A` 的长度。
- 空间复杂度：$O(1)$。