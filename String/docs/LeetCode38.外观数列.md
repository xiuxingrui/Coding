# [LeetCode38.外观数列](https://leetcode-cn.com/problems/count-and-say/)
## 题目描述
给定一个正整数 `n``(1 ≤ n ≤ 30)`，输出外观数列的第 `n` 项。

注意：整数序列中的每一项将表示为一个字符串。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

第一项是数字 1

描述前一项，这个数是 1 即 “一个 1 ”，记作 11

描述前一项，这个数是 11 即 “两个 1 ” ，记作 21

描述前一项，这个数是 21 即 “一个 2 一个 1 ” ，记作 1211

描述前一项，这个数是 1211 即 “一个 1 一个 2 两个 1 ” ，记作 111221


### 示例
```
输入: 1
输出: "1"
解释：这是一个基本样例。
```
```
输入: 4
输出: "1211"
解释：当 n = 3 时，序列是 "21"，其中我们有 "2" 和 "1" 两组，"2" 可以读作 "12"，也就是出现频次 = 1 而 值 = 2；类似 "1" 可以读作 "11"。所以答案是 "12" 和 "11" 组合在一起，也就是 "1211"。
```
## 题解
自己的解法
```java
class Solution {
    public String countAndSay(int n) {
        if(n==1){
            return String.valueOf(1);
        }
        String lastS=countAndSay(n-1);
        StringBuilder sb=new StringBuilder();
        char[] chs=lastS.toCharArray();
        int i=0;
        while(i<chs.length){
            int cnt=1;
            while(i+1<chs.length&&chs[i]==chs[i+1]){
                cnt++;
                i++;
            }
            // String temp=String.valueOf(cnt);
            // char[] t=temp.toCharArray();
            // for(int j=0;j<t.length;j++){
            //     sb.append(t[j]);
            // }
            sb.append(cnt).append(chs[i]);
            i++;
        }
        return sb.toString();
    }
}
```