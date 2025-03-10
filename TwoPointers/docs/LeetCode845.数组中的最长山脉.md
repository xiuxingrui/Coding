# [LeetCode845.数组中的最长山脉](https://leetcode-cn.com/problems/longest-mountain-in-array/)
## 题目描述
我们把数组 `A` 中符合下列属性的任意连续子数组 `B` 称为 “山脉”：

- `B.length >= 3`
- 存在 `0 < i < B.length - 1` 使得 `B[0] < B[1] < ... B[i-1] < B[i] > B[i+1] > ... > B[B.length - 1]`

（注意：`B` 可以是 `A` 的任意子数组，包括整个数组 `A`。）

给出一个整数数组 `A`，返回最长 “山脉” 的长度。

如果不含有 “山脉” 则返回 0。

- `0 <= A.length <= 10000`
- `0 <= A[i] <= 10000`

### 示例
```
输入：[2,1,4,7,3,2,5]
输出：5
解释：最长的 “山脉” 是 [1,4,7,3,2]，长度为 5。
```
```
输入：[2,2,2]
输出：0
解释：不含 “山脉”。
```
## 题解
对于一座「山脉」，我们称首元素 `B[0]` 和尾元素 `B[len(B)−1]` 为「山脚」，满足题目要求 `B[i−1]<B[i]>B[i+1]`的元素 `B[i]` 为「山顶」。为了找出数组 `A` 中最长的山脉，我们可以考虑枚举山顶，再从山顶向左右两侧扩展找到山脚。

由于从左侧山脚到山顶的序列是严格单调递增的，而从山顶到右侧山脚的序列是严格单调递减的，因此我们可以使用动态规划（也可以理解为递推）的方法，计算出从任意一个元素开始，向左右两侧最多可以扩展的元素数目。

我们用 `left[i]` 表示 `A[i]` 向左侧最多可以扩展的元素数目。如果 `A[i−1]<A[i]`，那么 `A[i]` 可以向左扩展到 `A[i−1]`，再扩展 `left[i]` 个元素，因此有 `left[i]=left[i−1]+1`

如果 `A[i−1]≥A[i]`，那么 `A[i]` 无法向左扩展，因此有`left[i]=0`

特别地，当 `i=0` 时，`A[i]` 为首元素，无法向左扩展，因此同样有`left[0]=0`

同理，我们用 `right[i]` 表示 `A[i]` 向右侧最多可以扩展的元素数目，那么有类似的状态转移方程（递推式）

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201025185642.png)

其中 `n` 是数组 `A` 的长度。

在计算出所有的 `left[]` 以及 `right[]` 之后，我们就可以枚举山顶。需要注意的是，只有当 `left[i]` 和 `right[i]` 均大于 0 时，`A[i]` 才能作为山顶，并且山脉的长度为 `left+right[i]+1`。

在所有的山脉中，最长的即为答案。
```java
class Solution {
    public int longestMountain(int[] A) {
        int n = A.length;
        if (n == 0) {
            return 0;
        }
        int[] left = new int[n];
        for (int i = 1; i < n; ++i) {
            left[i] = A[i - 1] < A[i] ? left[i - 1] + 1 : 0;
        }
        int[] right = new int[n];
        for (int i = n - 2; i >= 0; --i) {
            right[i] = A[i + 1] < A[i] ? right[i + 1] + 1 : 0;
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = Math.max(ans, left[i] + right[i] + 1);
            }
        }
        return ans;
    }
}
```

```java
class Solution {
    public int longestMountain(int[] A) {
        int len=A.length;
        if(len<3){
            return 0;
        }
        int maxLen=0;
        for(int i=1;i<len-1;i++){
            if(A[i]>A[i-1]&&A[i]>A[i+1]){
                int j=i-1;
                while(j>=0&&A[j]<A[j+1]){
                    j--;
                }
                int k=i+1;
                while(k<len&&A[k]<A[k-1]){
                    k++;
                }
                maxLen=Math.max(maxLen,k-j-1);
            }
        }
        return maxLen;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n)$,其中 `n` 是数组 `A` 的长度。
- 空间复杂度：$O(n)$，即为数组 `left` 和 `right` 需要使用的空间。