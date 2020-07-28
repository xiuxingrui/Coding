# [LeetCode410.分割数组的最大值](https://leetcode-cn.com/problems/split-array-largest-sum/)
## 题目描述
给定一个非负整数数组和一个整数 `m`，你需要将这个数组分成 `m` 个非空的连续子数组。设计一个算法使得这 `m` 个子数组各自和的最大值最小。

数组长度 `n` 满足以下条件:

- `1 ≤ n ≤ 1000`
- `1 ≤ m ≤ min(50, n)`

### 示例
```
输入:
nums = [7,2,5,10,8]
m = 2

输出:
18

解释:
一共有四种方法将nums分割为2个子数组。
其中最好的方式是将其分为[7,2,5] 和 [10,8]，
因为此时这两个子数组各自的和的最大值为18，在所有情况中最小。
```
## 题解
### 解法一:二分
当我们选定一个值 `x`，我们可以线性地验证是否存在一种分割方案，满足其最大分割子数组和不超过 `x`。策略如下：

贪心地模拟分割的过程，从前到后遍历数组，用 `sum` 表示当前分割子数组的和，`cnt` 表示已经分割出的子数组的数量（包括当前子数组），那么每当 `sum` 加上当前值超过了 `x`，我们就把当前取的值作为新的一段分割子数组的开头，并将 `cnt` 加 1。遍历结束后验证是否 `cnt` 不超过 `m`。

这样我们可以用二分查找来解决。二分的上界为数组 `nums` 中所有元素的和，下界为数组 `nums` 中所有元素的最大值。通过二分查找，我们可以得到最小的最大分割子数组和，这样就可以得到最终的答案了。


```java
class Solution {
    public int splitArray(int[] nums, int m) {
        int left = 0, right = 0;
        for (int i = 0; i < nums.length; i++) {
            right += nums[i];
            if (left < nums[i]) {
                left = nums[i];
            }
        }
        while (left < right) {
            int mid = (right - left) / 2 + left;
            if (check(nums, mid, m)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    public boolean check(int[] nums, int x, int m) {
        int sum = 0;
        int cnt = 1;
        for (int i = 0; i < nums.length; i++) {
            if (sum + nums[i] > x) {
                cnt++;
                sum = nums[i];
            } else {
                sum += nums[i];
            }
        }
        return cnt <= m;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n×log(sum−maxn))$，其中 `sum` 表示数组 `nums`中所有元素的和，`maxn` 表示数组所有元素的最大值。每次二分查找时，需要对数组进行一次遍历，时间复杂度为 $O(n)$，因此总时间复杂度是 $O(n×log(sum−maxn))$。

- 空间复杂度：$O(1)$。

