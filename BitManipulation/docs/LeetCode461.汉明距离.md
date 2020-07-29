# [LeetCode461.汉明距离](https://leetcode-cn.com/problems/hamming-distance/)
## 题目描述
两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 `x` 和 `y`，计算它们之间的汉明距离。

注意：
`0 ≤ x, y < 2^31`.

### 示例
```
输入: x = 1, y = 4

输出: 2

解释:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
上面的箭头指出了对应二进制位不同的位置。
```
## 题解
### 解法一
当我们在 `number` 和 `number-1` 上做 `AND` 位运算时，原数字 `number` 的最右边等于 1 的比特会被移除。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200729145458.png)

```java
class Solution {
  public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int distance = 0;
    while (xor != 0) {
      distance += 1;
      // remove the rightmost bit of '1'
      xor = xor & (xor - 1);
    }
    return distance;
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(1)$。
  - 与移位方法相似，由于整数的位数恒定，因此具有恒定的时间复杂度。
  - 但是该方法需要的迭代操作更少。
- 空间复杂度：$O(1)$，与输入无关，使用恒定大小的空间。
### 解法二
为了计算等于 1 的位数，可以将每个位移动到最左侧或最右侧，然后检查该位是否为 1。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200729145637.png)

这里采用右移位，每个位置都会被移动到最右边。移位后检查最右位的位是否为 1 即可。检查最右位是否为 1，可以使用取模运算（`i%2`）或者 `AND` 操作（`i&1`），这两个操作都会屏蔽最右位以外的其他位。
```java
class Solution {
  public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int distance = 0;
    while (xor != 0) {
      if (xor % 2 == 1)
        distance += 1;
      xor = xor >> 1;
    }
    return distance;
  }
}
```
#### 复杂度分析
- 时间复杂度:$O(1)$，在 `Python` 和 `Java` 中 `Integer` 的大小是固定的，处理时间也是固定的。 32 位整数需要 32 次迭代。
- 空间复杂度：$O(1)$，使用恒定大小的空间。
