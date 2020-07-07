# [LeetCode66.加一](https://leetcode-cn.com/problems/plus-one/)
## 题目描述
给定一个由**整数**组成的**非空**数组所表示的**非负**整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

### 示例
```
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。

输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。
```
## 题解
### 解法一
根据题意加一，没错就是加一这很重要，因为它是只加一的所以有可能的情况就只有两种：
- 除 9 之外的数字加一；
- 数字 9。

加一得十进一位个位数为 0 加法运算如不出现进位就运算结束了且进位只会是一。

所以只需要判断有没有进位并模拟出它的进位方式，如十位数加 1 个位数置为 0，如此循环直到判断没有再进位就退出循环返回结果。

然后还有一些特殊情况就是当出现 99、999 之类的数字时，循环到最后也需要进位，出现这种情况时需要手动将它进一位。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            digits[i]++;
            digits[i] = digits[i] % 10;
            if (digits[i] != 0) return digits;
        }
        digits = new int[digits.length + 1];
        digits[0] = 1;
        return digits;
    }
}
```
### 解法二
```java
class Solution {
    public int[] plusOne(int[] digits) {
        int flag=0;
        for(int i=digits.length-1;i>=0;--i){
            if(i==digits.length-1){
                digits[i]+=1+flag;
            }
            else{
                digits[i]+=flag;
            }
            if(digits[i]>9){
                digits[i]-=10;
                flag=1;
            }else{
                flag=0;
            }
        }
        if(flag==0){
            return digits;
        }
        else{
            int[] res=new int[digits.length+1];
            res[0]=1;
            res[1]=digits[0];
            for(int i=2;i<digits.length;++i){
                res[i]=digits[i];
            }
            return res;
        }
    }
}
```
