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
        int idx=0;
        boolean flag=true;
        while(flag==true){
            for(int i=0;i<len-1;i++){
                if(idx>=strs[i].length()||idx>=strs[i+1].length()){
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
### 解法二
纵向扫描：

纵向扫描时，从前往后遍历所有字符串的每一列，比较相同列上的字符是否相同，如果相同则继续对下一列进行比较，如果不相同则当前列不再属于公共前缀，当前列之前的部分为最长公共前缀。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200923162453.png)
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        int length = strs[0].length();
        int count = strs.length;
        for (int i = 0; i < length; i++) {
            char c = strs[0].charAt(i);
            for (int j = 1; j < count; j++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0];
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mn)$，其中 `m` 是字符串数组中的字符串的平均长度，`n` 是字符串的数量。最坏情况下，字符串数组中的每个字符串的每个字符都会被比较一次。
- 空间复杂度：$O(1)$。使用的额外空间复杂度为常数。
### 解法三
横向扫描：

依次遍历字符串数组中的每个字符串，对于每个遍历到的字符串，更新最长公共前缀，当遍历完所有的字符串以后，即可得到字符串数组中的最长公共前缀。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200923162301.png)

如果在尚未遍历完所有的字符串时，最长公共前缀已经是空串，则最长公共前缀一定是空串，因此不需要继续遍历剩下的字符串，直接返回空串即可。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        String prefix = strs[0];
        int count = strs.length;
        for (int i = 1; i < count; i++) {
            prefix = longestCommonPrefix(prefix, strs[i]);
            if (prefix.length() == 0) {
                break;
            }
        }
        return prefix;
    }

    public String longestCommonPrefix(String str1, String str2) {
        int length = Math.min(str1.length(), str2.length());
        int index = 0;
        while (index < length && str1.charAt(index) == str2.charAt(index)) {
            index++;
        }
        return str1.substring(0, index);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mn)$，其中 `m` 是字符串数组中的字符串的平均长度，`n` 是字符串的数量。最坏情况下，字符串数组中的每个字符串的每个字符都会被比较一次。
- 空间复杂度：$O(1)$。使用的额外空间复杂度为常数。

