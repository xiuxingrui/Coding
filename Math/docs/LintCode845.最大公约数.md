# [LintCode845.最大公约数](https://www.jiuzhang.com/solution/greatest-common-divisor/)
## 题目描述
给两个数字，数字 `a` 跟数字 `b`。找到两者的最大公约数。

### 样例
```
输入: a = 10, b = 15
输出: 5
解释:
10 % 5 == 0
15 % 5 == 0

输入: a = 15, b = 30
输出: 15
解释:
15 % 15 == 0
30 % 15 == 0
```
## 题解
```java
class Solution {
    /**
     * @param a: the given number
     * @param b: another number
     * @return: the greatest common divisor of two numbers
     */
    public int gcd(int a, int b) {
        return b==0?a:gcd(b, a % b);
    }
}
```