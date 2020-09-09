# [LeetCode14.最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)
## 题目描述
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

所有输入只包含小写字母 `a-z` 。
### 示例
```
输入: ["flower","flow","flight"]
输出: "fl"
```
```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```
## 题解
### 解法一
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int len=strs.length;
        if(len==0){
            return "";
        }
        if(len==1){
            return strs[0];
        }
        int[] lens=new int[len];
        for(int i=0;i<len;i++){
            lens[i]=strs[i].length();
        }
        int idx=0;
        boolean flag=true;
        while(flag==true){
            for(int i=0;i<len-1;i++){
                if(idx>=lens[i]||idx>=lens[i+1]){
                    flag=false;
                    break;
                }
                if(strs[i].charAt(idx)!=strs[i+1].charAt(idx)){
                    flag=false;
                    break;
                }
            }
            if(flag==true){
                idx++;
            }
        }
        return strs[0].substring(0,idx);
    }
}
```
