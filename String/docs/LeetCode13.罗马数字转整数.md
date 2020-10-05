# [LeetCode13.罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)
## 题目描述
罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。
```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X + II` 。 27 写做  `XXVII`, 即为 `XX + V + II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

- 题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
- `IC` 和 `IM` 这样的例子并不符合题目要求，49 应该写作 `XLIX`，999 应该写作 `CMXCIX` 。

### 示例
```
输入: "III"
输出: 3
```
```
输入: "IV"
输出: 4
```
```
输入: "IX"
输出: 9
```
```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```
```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```
## 题解
按照题目的描述，可以总结如下规则：

- 罗马数字由 `I`,`V`,`X`,`L`,`C`,`D`,`M` 构成；
- 当小值在大值的左边，则减小值，如 `IV`=5-1=4；
- 当小值在大值的右边，则加小值，如 `VI`=5+1=6；
由上可知，右值永远为正，因此最后一位必然为正。

一言蔽之，把一个小值放在大值的左边，就是做减法，否则为加法。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201004212402.png)

```java
class Solution {
    public int romanToInt(String s) {
        char[] arr=s.toCharArray();
        HashMap<Character,Integer> map=new HashMap<>(){{
            put('I',1);
            put('V',5);
            put('X',10);
            put('L',50);
            put('C',100);
            put('D',500);
            put('M',1000);
        }};
        int i=0,ans=0;
        while(i<s.length()){
            int value=map.get(arr[i]);
            if(i<s.length()-1&&value<map.get(arr[i+1])){
                ans-=value;
            }else{
                ans+=value;
            }
            i++;
        }
        return ans;
    }
}
```
自己的解法：
```java
class Solution {
    public int romanToInt(String s) {
        char[] arr=s.toCharArray();
        HashMap<Character,Integer> map=new HashMap<>(){{
            put('I',1);
            put('V',5);
            put('X',10);
            put('L',50);
            put('C',100);
            put('D',500);
            put('M',1000);
        }};
        int i=0,ans=0;
        while(i<s.length()){
            if(arr[i]=='I'){
                if(i<s.length()-1&&arr[i+1]=='V'){
                    ans+=4;
                    i+=2;
                }else if(i<s.length()-1&&arr[i+1]=='X'){
                    ans+=9;
                    i+=2;
                }else{
                    ans+=1;
                    i++;
                }
            }else if(arr[i]=='X'){
                if(i<s.length()-1&&arr[i+1]=='L'){
                    ans+=40;
                    i+=2;
                }else if(i<s.length()-1&&arr[i+1]=='C'){
                    ans+=90;
                    i+=2;
                }else{
                    ans+=10;
                    i++;
                }
            }else if(arr[i]=='C'){
                if(i<s.length()-1&&arr[i+1]=='D'){
                    ans+=400;
                    i+=2;
                }else if(i<s.length()-1&&arr[i+1]=='M'){
                    ans+=900;
                    i+=2;
                }else{
                    ans+=100;
                    i++;
                }
            }else{
                ans+=map.get(arr[i]);
                i++;
            }
        }
        return ans;
    }
}
```
