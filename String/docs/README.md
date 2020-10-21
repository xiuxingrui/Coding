# [LeetCode1071.字符串的最大公因子](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)
## 题解
对于字符串 `S` 和 `T`，只有在 `S = T + ... + T`（`T` 与自身连接 1 次或多次）时，我们才认定 “`T` 能除尽 `S`”。

返回最长字符串 `X`，要求满足 `X` 能除尽 `str1` 且 `X` 能除尽 `str2`。

- `1 <= str1.length <= 1000`
- `1 <= str2.length <= 1000`
- `str1[i]`  和 `str2[i]` 为大写英文字母
### 示例
```
输入：str1 = "ABCABC", str2 = "ABC"
输出："ABC"
``
```
输入：str1 = "ABABAB", str2 = "ABAB"
输出："AB"
```
``
输入：str1 = "LEET", str2 = "CODE"
输出：""
```
## 题解
### 解法一
首先答案肯定是字符串的某个前缀，然后简单直观的想法就是枚举所有的前缀来判断，我们设这个前缀串长度为 `lenx`，`str1` 的长度为 `len1`，`str2` 的长度为 `len2`，则我们知道前缀串的长度必然要是两个字符串长度的约数才能满足条件，否则无法经过若干次拼接后得到长度相等的字符串，公式化来说，即

- `len1%lenx==0`
- `len2%lenx==0`

所以我们可以枚举符合长度条件的前缀串，再去判断这个前缀串拼接若干次以后是否等于 `str1` 和 `str2` 即可。

由于题目要求最长的符合要求的字符串 `X`，所以可以按长度从大到小枚举前缀串，这样碰到第一个满足条件的前缀串返回即可。
```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        int len1 = str1.length(), len2 = str2.length();
        for (int i = Math.min(len1, len2); i >= 1; --i) { // 从长度大的开始枚举
            if (len1 % i == 0 && len2 % i == 0) {
                String X = str1.substring(0, i);
                if (check(X, str1) && check(X, str2)) {
                    return X;
                }
            }
        }
        return "";
    }

    public boolean check(String t, String s) {
        int lenx = s.length() / t.length();
        StringBuilder ans = new StringBuilder();
        for (int i = 1; i <= lenx; ++i) {
            ans.append(t);
        }
        return ans.toString().equals(s);
    }
}
```
自己的写法：
```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        int idx=0,l1=str1.length(),l2=str2.length();
        char[] arr1=str1.toCharArray();
        char[] arr2=str2.toCharArray();
        while(idx<l1&&idx<l2){
            if(arr1[idx]!=arr2[idx]){
                break;
            }
            idx++;
        }
        for(int i=1;i<=idx;i++){
            if(idx%i==0){
                int pos=idx/i;
                if(helper(arr1,pos)&&helper(arr2,pos)){
                    return str1.substring(0,pos);
                }
            }
        }
        return "";
    }
    public boolean helper(char[] arr,int idx){
        int len=idx;
        if(arr.length%len!=0){
            return false;
        }
        for(int i=0;i<idx;i++){
            for(int j=i;j<arr.length;j+=len){
                if(arr[j]!=arr[i]){
                    return false;
                }
            }
        }
        return true;
    }
}
```
#### 复杂度分析
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201020190042.png)
### 解法二
注意到一个性质：如果存在一个符合要求的字符串 `X`，那么也一定存在一个符合要求的字符串 `X'`，它的长度为 `str1` 和 `str2` 长度的最大公约数。


