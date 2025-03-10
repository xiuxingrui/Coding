# [LeetCode69.x的平方根](https://leetcode-cn.com/problems/sqrtx/)
## 题目描述
实现 `int sqrt(int x)` 函数。

计算并返回 `x` 的平方根，其中 `x` 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

### 示例
```
输入: 4
输出: 2
```
```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```
## 题解
考虑`x=0`和`x=1`两种特殊情况。
```java
class Solution {
    public int mySqrt(int x) {
        if(x==0||x==1){
            return x;
        }
        int left=0,right=x;
        while(left<right){
            int mid=left+(right-left)/2;
            if((long)mid*mid>x){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        return left-1;
    }
}
```
另外一种思路，一个数的平方根应该不会超过`x/2+1`
```java
class Solution {
    public int mySqrt(int x) {
        int left=0,right=x/2+2;
        while(left<right){
            int mid=left+(right-left)/2;
            if((long)mid*mid>x){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        return left-1;
    }
}
```
