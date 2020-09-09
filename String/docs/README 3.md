# [剑指Offer48.最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)
## 题目描述
请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

### 示例
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
## 题解
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashSet<Character> set=new HashSet<>();
        char[] charArray=s.toCharArray();
        int len=s.length();
        int ans=0;
        for(int i=0;i<len;i++){
            set.clear();
            int tmepLen=0;
            for(int j=i;j<len;j++){
                if(set.contains(charArray[j])==true){
                    break;
                }
                tmepLen++;
                ans=Math.max(ans,tmepLen);
                set.add(charArray[j]);
            }
        }
        return ans;
    }
}
```