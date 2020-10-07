# [LeetCode400.第N个数字](https://leetcode-cn.com/problems/nth-digit/)
## 题目描述
在无限的整数序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...中找到第 `n` 个数字。

注意:

`n` 是正数且在32位整数范围内 ( `n < 2^31`)。
### 示例
```
输入:
3

输出:
3
```
```
输入:
11

输出:
0
说明:
第11个数字在序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... 里是0，它是10的一部分。
```
## 题解
- 将 101112⋯中的每一位称为 数位 ，记为 `n` ；
- 将 10,11,12,⋯10, 11, 12,⋯ 称为 数字 ，记为 `num` ；
- 数字 10 是一个两位数，称此数字的 位数 为 2 ，记为 `digit` ；
- 每 `digit` 位数的起始数字（即：1,10,100,⋯），记为 `start` 。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007165118.png)

观察上表，可推出各 `digit` 下的数位数量 `count` 的计算公式：`count=9×start×digit`

根据以上分析，可将求解分为三步：

- 确定 `n` 所在 数字 的 位数 ，记为 `digit` ；
- 确定 `n` 所在的 数字 ，记为 `num` ；
- 确定 `n` 是 `num` 中的哪一数位，并返回结果。

1. 确定所求数位的所在数字的位数

如下图所示，循环执行 `n` 减去 一位数、两位数、... 的数位数量 `count` ，直至 `n≤count` 时跳出。

由于 `n` 已经减去了一位数、两位数、...、`(digit−1)` 位数的 数位数量 `count` ，因而此时的 `n` 是从起始数字 `start` 开始计数的。

```python
digit, start, count = 1, 1, 9
while n > count:
    n -= count
    start *= 10 # 1, 10, 100, ...
    digit += 1  # 1,  2,  3, ...
    count = 9 * start * digit # 9, 180, 2700, ...
```
结论： 所求数位 ① 在某个 `digit` 位数中； ② 为从数字 `start` 开始的第 `n` 个数位。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007165411.png)

2. 确定所求数位所在的数字

3. 确定所求数位在 `num` 的哪一数位

比如输入的 `n` 是 365：

- 经过第一步计算我们可以得到第 365 个数字表示的数是三位数，`n=365−9−90×2=176`，`digtis = 3`。这时 `n=176` 表示目标数字是三位数中的第 176 个数字。
- 我们设目标数字所在的数为 `number`，计算得到 `number=100+176/3=158`，`idx` 是目标数字在 `number` 中的索引，如果 `idx = 0`，表示目标数字是 `number` 中的最后一个数字。
- 我们可以计算得到 `idx = n % digits = 176 % 3 = 2`，说明目标数字应该是 `number = 158` 中的第二个数字，即输出为 5。
```java
class Solution {
    public int findNthDigit(int n) {
        int digit = 1;
        long start = 1;
        long count = 9;
        while (n > count) { 
            n -= count;
            digit += 1;
            start *= 10;
            count = digit * start * 9;
        }
        int idx=n%digit;
        if(idx==0){
            idx=digit;
        }
        start+=idx==digit?n/digit-1:n/digit;
        return String.valueOf(start).charAt(idx-1)-'0';
    }
}
```
### 复杂度分析：
- 时间复杂度 $O(logn)$ ： 所求数位 `n` 对应数字 `num` 的位数 `digit` 最大为 $O(logn)$ ；第一步最多循环 $O(logn)$ 次；第三步中将 `num` 转化为字符串使用 $O(logn)$ 时间；因此总体为 $O(logn)$ 。
- 空间复杂度 $O(logn)$ ： 将数字 `num` 转化为字符串 `str(num)` ，占用 $O(logn)$ 的额外空间。
