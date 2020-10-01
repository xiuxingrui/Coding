# [LeetCode151.翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)
## 题目描述
给定一个字符串，逐个翻转字符串中的每个单词。

### 示例
```
输入: "the sky is blue"
输出: "blue is sky the"
```
```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```
```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```
## 题解
```java
class Solution {
    public String reverseWords(String s) {
        StringBuilder sb=new StringBuilder();
        s=s.trim();
        char[] cs=s.toCharArray();
        int len=cs.length,i=0;
        Deque<Character> stack=new LinkedList<>();
        Deque<Character> temp=new LinkedList<>();
        while(i<len){
            if(cs[i]==' '){
                stack.push(' ');
                while(i<len&&cs[i]==' '){
                    i++;
                }
            }
            while(i<len&&cs[i]!=' '){
                temp.push(cs[i]);
                i++;
            }
            while(!temp.isEmpty()){
                stack.push(temp.pop());
            }
        }
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        return sb.toString();
    }
}
```