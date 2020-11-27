# [LeetCode454.四数相加II](https://leetcode-cn.com/problems/4sum-ii/)
## 题目描述
给定四个包含整数的数组列表 `A` , `B` , `C` , `D` ,计算有多少个元组 `(i, j, k, l)` ，使得 `A[i] + B[j] + C[k] + D[l] = 0`。

为了使问题简单化，所有的 `A`, `B`, `C`, `D` 具有相同的长度 `N`，且 `0 ≤ N ≤ 500` 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

### 示例
```
输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

输出:
2

解释:
两个元组如下:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```
## 题解
我们可以将四个数组分成两部分，`A` 和 `B` 为一组，`C` 和 `D` 为另外一组。

对于 `A` 和 `B`，我们使用二重循环对它们进行遍历，得到所有 `A[i]+B[j]` 的值并存入哈希映射中。对于哈希映射中的每个键值对，每个键表示一种 `A[i]+B[j]`，对应的值为 `A[i]+B[j]` 出现的次数。

对于 `C` 和 `D`，我们同样使用二重循环对它们进行遍历。当遍历到 `C[k]+D[l]` 时，如果 `−(C[k]+D[l])` 出现在哈希映射中，那么将 `−(C[k]+D[l])` 对应的值累加进答案中。

最终即可得到满足 `A[i]+B[j]+C[k]+D[l]=0` 的四元组数目。

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        int cnt=0;
        Map<Integer,Integer> countAB=new HashMap<>();
        for(int numA:A){
            for(int numB:B){
                countAB.put(numA+numB,countAB.getOrDefault(numA+numB,0)+1);
            }
        }
        for(int numC:C){
            for(int numD:D){
                if(countAB.containsKey(-numC-numD)){
                    cnt+=countAB.get(-numC-numD);
                }
            }
        }
        return cnt;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n^2)$。我们使用了两次二重循环，时间复杂度均为 $O(n^2)$。在循环中对哈希映射进行的修改以及查询操作的期望时间复杂度均为 $O(1)$，因此总时间复杂度为 $O(n^2)$。
- 空间复杂度：$O(n^2)$，即为哈希映射需要使用的空间。在最坏的情况下，`A[i]+B[j]` 的值均不相同，因此值的个数为 `n^2`，也就需要 $O(n^2)$的空间。
