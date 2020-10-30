# [LeetCode389.找不同](https://leetcode-cn.com/problems/find-the-difference/)
## 题解
给定两个字符串 `s` 和 `t`，它们只包含小写字母。

字符串 `t` 由字符串 `s` 随机重排，然后在随机位置添加一个字母。

请找出在 `t` 中被添加的字母。

- `0 <= s.length <= 1000`
- `t.length == s.length + 1`
- `s` 和 `t` 只包含小写字母
### 示例
```
输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。
```
```
输入：s = "", t = "y"
输出："y"
```
```
输入：s = "a", t = "aa"
输出："a"
```
```
输入：s = "ae", t = "aea"
输出："a"
```
## 题解
### 解法一
相同字符异或操作以后为0，最后会得到被添加的字母
```java
class Solution {
    public char findTheDifference(String s, String t) {
        char res = 0;
        int lens = s.length();
        char[] arr1=s.toCharArray();
        char[] arr2=t.toCharArray();
        for (int i = 0; i < lens; i ++) {
            res ^= arr1[i]^ arr2[i];
        }
        res ^= arr2[lens];
        return res;
    }
}
```
### 解法二
```java
class Solution {
    public char findTheDifference(String s, String t) {
        int[] cnt1=new int[26];
        int[] cnt2=new int[26];
        char[] arr1=s.toCharArray();
        char[] arr2=t.toCharArray();
        for(char c:arr1){
            cnt1[c-'a']++;
        }
        for(char c:arr2){
            if(cnt1[c-'a']==0){
                return c;
            }
            cnt2[c-'a']++;
            if(cnt2[c-'a']==cnt1[c-'a']+1){
                return c;
            }
        }
        return 'a';
    }
}
```