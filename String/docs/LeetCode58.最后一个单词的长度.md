# [LeetCode58.最后一个单词的长度](https://leetcode-cn.com/problems/length-of-last-word/)
## 题目描述
给定一个仅包含大小写字母和空格 `' '` 的字符串 `s`，返回其最后一个单词的长度。如果字符串从左向右滚动显示，那么最后一个单词就是最后出现的单词。

如果不存在最后一个单词，请返回 0 。

说明：一个单词是指仅由字母组成、不包含任何空格字符的 最大子字符串。

### 示例
```java
输入: "Hello World"
输出: 5
```
## 题解
标签：字符串遍历
- 从字符串末尾开始向前遍历，其中主要有两种情况
- 第一种情况，以字符串`"Hello World"`为例，从后向前遍历直到遍历到头或者遇到空格为止，即为最后一个单词`"World"`的长度5
- 第二种情况，以字符串`"Hello World "`为例，需要先将末尾的空格过滤掉，再进行第一种情况的操作，即认为最后一个单词为`"World"`，长度为5
所以完整过程为先从后过滤掉空格找到单词尾部，再从尾部向前遍历，找到单词头部，最后两者相减，即为单词的长度

时间复杂度：$O(n)$，`n`为结尾空格和结尾单词总体长度

```java
class Solution {
    public int lengthOfLastWord(String s) {
        int end = s.length() - 1;
        while(end >= 0 && s.charAt(end) == ' ') end--;
        if(end < 0) return 0;
        int start = end;
        while(start >= 0 && s.charAt(start) != ' ') start--;
        return end - start;
    }
}
```
自己解法:
```java
class Solution {
    public int lengthOfLastWord(String s) {
        int len=s.length();
        char[] charArray=s.toCharArray();
        for(int i=len-1;i>=0;i--){
            if(charArray[i]!=' '){
                break;
            }else{
                len--;
            }
            
        }
        if(len==0){
            return 0;
        }
        int index=-1;
        for(int i=0;i<len;i++){
            if(charArray[i]==' '){
                index=i;
            }
        }
        if(index==-1){
            return len;
        }else{
            return len-1-index;
        }
    }
}
```