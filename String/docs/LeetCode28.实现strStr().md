# [LeetCode28.实现strStr()](https://leetcode-cn.com/problems/implement-strstr/)
## 题目描述
实现 `strStr()` 函数。

给定一个 `haystack` 字符串和一个 `needle` 字符串，在 `haystack` 字符串中找出 `needle` 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 `strstr()` 以及 `Java`的 `indexOf()` 定义相符。

### 示例
```
输入: haystack = "hello", needle = "ll"
输出: 2
示例 2:
```
```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```
## 题解
### 解法一
`KMP`

`next[j]`的含义就是`j+1`位失配时，`j`应当回到的位置，也是子串 `s[0...j]`的最长相等前后缀的前缀最后一位的下标。
```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.equals("")){
            return 0;
        }
        char[] s=haystack.toCharArray();
        char[] t=needle.toCharArray();
        int[] next=getNext(t);
        int j=-1;
        for(int i=0;i<haystack.length();i++){
            while(j!=-1&&s[i]!=t[j+1]){
                j=next[j];
            }
            if(s[i]==t[j+1]){
                j++;
            }
            if(j==needle.length()-1){
                return i-needle.length()+1;
            }
        }
        return -1;
    }
    public int[] getNext(char[] s){
        int len=s.length;
        int[] next=new int[len];
        int j=-1;
        next[0]=j;
        for(int i=1;i<len;i++){
            while(j!=-1&&s[i]!=s[j+1]){
                j=next[j];
            }
            if(s[i]==s[j+1]){
                j++;
            }
            next[i]=j;
        }
        return next;
    }
}
```
优化`KMP`:

`nextVal[i]`的含义应理解为当模式串`pattern`的`i+1`位发生失配时，`i`应当回退到的最佳位置。
```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.equals("")){
            return 0;
        }
        char[] s=haystack.toCharArray();
        char[] t=needle.toCharArray();
        int[] nextVal=getNextVal(t);
        int j=-1;
        for(int i=0;i<haystack.length();i++){
            while(j!=-1&&s[i]!=t[j+1]){
                j=nextVal[j];
            }
            if(s[i]==t[j+1]){
                j++;
            }
            if(j==needle.length()-1){
                return i-needle.length()+1;
            }
        }
        return -1;
    }
    public int[] getNextVal(char[] s){
        int len=s.length;
        int[] nextVal=new int[len];
        int j=-1;
        nextVal[0]=j;
        for(int i=1;i<len-1;i++){//nextVal[len-1]没用
            while(j!=-1&&s[i]!=s[j+1]){
                j=nextVal[j];
            }
            if(s[i]==s[j+1]){
                j++;
            }
            if(j==-1||s[i+1]!=s[j+1]){
                nextVal[i]=j;
            }else{
                nextVal[i]=nextVal[j];
            }
        }
        return nextVal;
    }
}
```
### 解法二
内置函数
```java
class Solution {
  public int strStr(String haystack, String needle) {
    int L = needle.length(), n = haystack.length();

    for (int start = 0; start < n - L + 1; ++start) {
      if (haystack.substring(start, start + L).equals(needle)) {
        return start;
      }
    }
    return -1;
  }
}
```
或
```java
class Solution {
  public int strStr(String haystack, String needle) {
      return haystack.indexOf(needle);
  }
}
```
暴力匹配：
```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.equals("")){
            return 0;
        }
        char[] s=haystack.toCharArray();
        char[] t=needle.toCharArray();
        for(int i=0;i<=haystack.length();i++){
            if(s[i]==t[0]){
                int j=0;
                for(int k=i;k<haystack.length()&&j<needle.length();k++){
                    if(s[k]!=t[j]){
                        break;
                    }
                    j++;
                }
                if(j==needle.length()){
                    return i;
                }
            }
        }
        return -1;
    }
}
```
优化：
```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.equals("")){
            return 0;
        }
        char[] s=haystack.toCharArray();
        char[] t=needle.toCharArray();
        for(int i=0;i<=haystack.length()-needle.length();i++){
            if(s[i]==t[0]){
                int j=0;
                for(int k=i;j<needle.length();k++){
                    if(s[k]!=t[j]){
                        break;
                    }
                    j++;
                }
                if(j==needle.length()){
                    return i;
                }
            }
        }
        return -1;
    }
}
```
