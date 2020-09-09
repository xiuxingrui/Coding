# [LeetCode125.验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)
## 题目描述
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

### 示例
```
输入: "A man, a plan, a canal: Panama"
输出: true
```
```
输入: "race a car"
输出: false
```
## 题解
### 解法一:库函数
```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuilder sb=new StringBuilder();
        for(int i=0;i<s.length();++i){
            char ch=s.charAt(i);
            if(Character.isLetterOrDigit(ch)){
                sb.append(Character.toLowerCase(ch));
            }
        }
        StringBuilder sbRev=new StringBuilder(sb).reverse();
        return sb.toString().equals(sbRev.toString());
    }
}
```
### 解法二:双指针
我们直接在原字符串 `s` 上使用双指针。在移动任意一个指针时，需要不断地向另一指针的方向移动，直到遇到一个字母或数字字符，或者两指针重合为止。也就是说，我们每次将指针移到下一个字母字符或数字字符，再判断这两个指针指向的字符是否相同。
```java
class Solution {
    public boolean isPalindrome(String s) {
        int i=0,j=s.length()-1;
        while(i<j){
            while(i<j&&!Character.isLetterOrDigit(s.charAt(i))){
                i++;
            }
            while(i<j&&!Character.isLetterOrDigit(s.charAt(j))){
                j--;
            }
            if(Character.toLowerCase(s.charAt(i))!=Character.toLowerCase(s.charAt(j))){
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```