# [LeetCode263.丑数](https://leetcode-cn.com/problems/ugly-number/)
## 题目描述
编写一个程序判断给定的数是否为丑数。

丑数就是只包含质因数 2, 3, 5 的**正整数**。

说明：

1. 1 是丑数。
2. 输入不会超过 32 位有符号整数的范围。

### 示例
```
输入: 6
输出: true
解释: 6 = 2 × 3

输入: 8
输出: true
解释: 8 = 2 × 2 × 2

输入: 14
输出: false 
解释: 14 不是丑数，因为它包含了另外一个质因数 7。
```
## 题解
```java
class Solution {
    public boolean isUgly(int num) {
        if(num==0) return false;
        while(num%2==0){
            num/=2;
        }
        while(num%3==0){
            num/=3;
        }
        while(num%5==0){
            num/=5;
        }
        if(num==1){
            return true;
        }else{
            return false;
        }
    }
}
```

