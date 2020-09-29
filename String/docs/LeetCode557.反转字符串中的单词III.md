# [LeetCode557.反转字符串中的单词III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)
## 题目描述
给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。
### 示例
```
输入："Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
```
## 题解
```java
class Solution {
    public String reverseWords(String s) {
        char[] chs=s.toCharArray();
        int len=chs.length;
        int i = 0;
        while (i < len) {
            int start = i;
            while (i < len && chs[i] != ' ') {
                i++;
            }
            int left = start, right = i - 1;
            while (left < right) {
                char temp=chs[left];
                chs[left]=chs[right];
                chs[right]=temp;
                left++;
                right--;
            }
            while (i < len && chs[i] == ' ') {
                i++;
            }
        }
        return new String(chs);
    }
}
```
自己的写法
```java
class Solution {
    public String reverseWords(String s) {
        char[] chs=s.toCharArray();
        int len=chs.length;
        int i=0;
        while(i<len){
            while(i<len&&chs[i]==' '){
                i++;
            }
            int j=i;
            while(j+1<len&&chs[j+1]!=' '){
                j++;
            }
            if(j<len){
                int left=i,right=j;
                while(left<right){
                    char temp=chs[left];
                    chs[left]=chs[right];
                    chs[right]=temp;
                    left++;
                    right--;
                }
            }
            i=j+1;
        }
        return new String(chs);
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N)$。字符串中的每个字符要么在 $O(1)$ 的时间内被交换到相应的位置，要么因为是空格而保持不动。
- 空间复杂度：$O(1)$。因为不需要开辟额外的数组。