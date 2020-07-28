# [LeetCode50.Pow(x,n)](https://leetcode-cn.com/problems/powx-n/)
## 题目描述
实现 `pow(x,n)` ，即计算 `x` 的 `n` 次幂函数。

- `-100.0 < x < 100.0`
- `n` 是 32 位有符号整数，其数值范围是 `[−2^31,2^31−1]` 。
### 示例
```
输入: 2.00000, 10
输出: 1024.00000
```
```
输入: 2.10000, 3
输出: 9.26100
```
```
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```
## 题解
### 解法一:快速幂+递归
```java
class Solution {
    public double quickMul(double x, long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }

    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(logn)$，即为递归的层数。
- 空间复杂度：$O(logn)$，即为递归的层数。这是由于递归的函数调用会使用栈空间。

### 解法二:快速幂+迭代
对于任何十进制正整数 `n` ，设其二进制为`$b_{m} \ldots b_{3} b_{2} b_{1}$`为二进制某位值，$i \in[1, m]$），则有：
1. 二进制转十进制：$n=1 b_{1}+2 b_{2}+4 b_{3}+\ldots+2^{m-1} b_{m}$
2. 幂的二进制展开：$x^{n}=x^{1 b_{1}+2 b_{2}+4 b_{3}+\ldots+2^{m-1} b_{m}}=x^{1 b_{1}} x^{2 b_{2}} x^{4 b_{3}} \ldots x^{2^{m-1} b_{m}}$

根据以上推导，可把计算$x^{n}$转化为解决以下两个问题：
1. 计算$x^{1}, x^{2}, x^{4}, \ldots, x^{2^{m-1}}$的值： 循环赋值操作$x=x^{2}$即可；
2. 获取二进制各位$b_{1}, b_{2}, b_{3}, \ldots, b_{m}$， 循环执行以下操作即可。
   1. $n \& 1$（与操作）： 判断 `n` 二进制最右一位是否为 1,取余数 `n%2`等价于 判断二进制最右位$n \& 1$ 
   2. $n>>1$（移位操作）： `n` 右移一位（可理解为删除最后一位），向下整除 $n//2$ 等价于 右移一位 $n>>1$ ；

```java
class Solution {
    double quickMul(double x, long N) {
        double ans = 1.0;
        // 贡献的初始值为 x
        double x_contribute = x;
        // 在对 N 进行二进制拆分的同时计算答案
        while (N > 0) {
            if (N % 2 == 1) {//if((N&1)==1){
                // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
                ans *= x_contribute;
            }
            // 将贡献不断地平方
            x_contribute *= x_contribute;
            // 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
            N /= 2;//N>>=1;
        }
        return ans;
    }

    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
}
```
#### 复杂度分析
时间复杂度$O(logn)$ ： 二分的时间复杂度为对数级别。
空间复杂度 $O(1)$ ： `res`, `b` 等变量占用常数大小额外空间。





