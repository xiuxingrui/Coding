# [LeetCode680.验证回文字符串Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)
## 题目描述
给定一个非空字符串 `s`，最多删除一个字符。判断是否能成为回文字符串。

字符串只包含从 `a-z` 的小写字母。字符串的最大长度是50000。

### 示例
```
输入: "aba"
输出: True
```
```
输入: "abca"
输出: True
解释: 你可以删除c字符。
```
## 题解
回文串的特点是左右对称。假如有两个指针从字符串的两端同时向中间走：如果遇到的元素相等，则该相等的元素是最终回文字符串的一部分；如果遇到的元素不等，则认为此时遇到了构建回文字符串的「障碍」，应当进行处理，处理方式见下文。

当左右两个指针遇到不等的元素时，按照题目最原本的意思，我们处理的方式是删除 左指针指向的字符 或者 右指针指向的字符，判断 剩余的所有字符 是否可以构成回文串。

我们观察一下题目给出的示例 2：
```
输入: "abca"
输出: True
解释: 你可以删除c字符。
```
如果左右指针从两端同时向中间走，那么：
```
第一步：
a       b       c       a
|                       |
left                  right
```
第二步：
```
a       b       c       a
        |       |
        left  right
```
第一步，左右指针遇到的元素相等，继续向中间走；

第二步，左右指针遇到的元素不等，则必须进行处理：我们必须删除其中的一个字符，然后再判断 剩余的所有字符 是否是回文串。
```
删除 b：
a       c       a
```
```
或者，  删除 c：
a       b       a
```
即判断 `aca` 或者 `aba` 是否为回文字符串。

如果删除一个字符后，剩余的全部字符构成字符串 是回文字符串，那么就满足题意。

```java
class Solution {
    public boolean validPalindrome(String s) {
        char[] chs=s.toCharArray();
        int i=0,j=s.length()-1;
        boolean flag=true;
        while(i<j){
            if(chs[i]!=chs[j]){
                return isPalindrome(chs,i+1,j)||isPalindrome(chs,i,j-1);
            }
            i++;
            j--;
        }
        return true;
    }
    public boolean isPalindrome(char[] chs,int i,int j){
        while(i<j){
            if(chs[i]!=chs[j]){
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```
### 复杂度分析

- 时间复杂度：$O(n)$，其中 `n` 是字符串的长度。判断整个字符串是否是回文字符串的时间复杂度是 $O(n)$，遇到不同字符时，判断两个子串是否是回文字符串的时间复杂度也都是 $O(n)$。
- 空间复杂度：$O(1)$。只需要维护有限的常量空间。