# [LeetCode976.三角形的最大周长](https://leetcode-cn.com/problems/largest-perimeter-triangle/)
## 题目描述
给定由一些正数（代表长度）组成的数组 `A`，返回由其中三个长度组成的、面积不为零的三角形的最大周长。

如果不能形成任何面积不为零的三角形，返回 0。

提示：
1. `3 <= A.length <= 10000`
2. `1 <= A[i] <= 10^6`

### 示例
```
输入：[2,1,2]
输出：5
```
```
输入：[1,2,1]
输出：0
```
```
输入：[3,2,3,4]
输出：10
```
```
输入：[3,6,2,3]
输出：8
```
## 题解
不失一般性的，我们假设三角形的边长满足 `a≤b≤c`。那么这三条边组成三角形的面积非零的充分必要条件是 `a+b>c`。

再假设我们已经知道 `c` 的长度了，我们没有理由不从数组中选择尽可能大的 `a` 与 `b`。因为当且仅当 `a+b>c`的时候，它们才能组成一个三角形。

基于这种想法，一个简单的算法就呼之欲出：排序数组。对于数组内任意的 `c`，我们选择满足条件的最大的 `a≤b≤c`，也就是大到小排序，位于 `c` 后面的两个元素。 从大到小枚举 `c`，如果能组成三角形的话，我们就返回答案。

```java
class Solution {
    public int largestPerimeter(int[] A) {
        Arrays.sort(A);
        for(int i=A.length-3;i>=0;i--){
            if(A[i]+A[i+1]>A[i+2]){
                return A[i]+A[i+1]+A[i+2];
            }
        }
        return 0;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(NlogN)$，其中 `N` 是数组 `A` 的长度。
- 空间复杂度：$O(1)$。



