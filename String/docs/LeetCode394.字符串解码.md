# [LeetCode394.字符串解码](https://leetcode-cn.com/problems/decode-string/)
## 题目描述
给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

### 示例
```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```
```
输入：s = "3[a2[c]]"
输出："accaccacc"
```
```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```
```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```
## 题解
本题难点在于括号内嵌套括号，需要从内向外生成与拼接字符串，这与栈的先入后出特性对应。

算法流程：

1. 构建辅助栈`stack`，遍历字符串 `s` 中每个字符 `c`；
   - 当`c`为数字时，将数字字符转化为数字 `multi`，用于后续倍数计算；
   - 当 `c` 为字母时，在 `res` 尾部添加 `c`；
   - 当 `c` 为 `[` 时，将当前 `multi` 和 `res` 入栈，并分别置空置 0：
     - 记录此 `[` 前的临时结果 `res` 至栈，用于发现对应 `]` 后的拼接操作；
     - 记录此 `[` 前的倍数 `multi` 至栈，用于发现对应 `]` 后，获取 `multi × [...]` 字符串。
     - 进入到新 `[` 后，`res` 和 `multi` 重新记录。
   - 当 `c` 为 `]` 时，`stack` 出栈，拼接字符串 `res = last_res + cur_multi * res`，其中:
     - `last_res`是上个 `[` 到当前 `[` 的字符串，例如 `"3[a2[c]]"` 中的 `a`；
     - `cur_multi`是当前 `[` 到 `]` 内字符串的重复倍数，例如 `"3[a2[c]]"` 中的 2。
2. 返回字符串 `res`。
```java
class Solution {
    public String decodeString(String s) {
        StringBuilder res = new StringBuilder();
        int multi = 0;
        Deque<Integer> stackMulti = new LinkedList<>();
        Deque<String> stackStr = new LinkedList<>();
        char[] arr=s.toCharArray();
        for(Character c : arr) {
            if(c == '[') {
                stackMulti.push(multi);
                stackStr.push(res.toString());
                multi = 0;
                res = new StringBuilder();
            }
            else if(c == ']') {
                StringBuilder tmp = new StringBuilder();
                int cnt = stackMulti.pop();
                for(int i = 0; i < cnt; i++){
                    tmp.append(res);
                }
                res = new StringBuilder(stackStr.pop() + tmp);
            }
            else if(Character.isDigit(c)) {
                multi = multi * 10 + c-'0';
            }
            else res.append(c);
        }
        return res.toString();
    }
}
```
自己的解法：
```java
class Solution {
    public String decodeString(String s) {
        StringBuilder ans=new StringBuilder();
        char[] arr=s.toCharArray();
        Deque<Character> stack1=new LinkedList<>();
        Deque<Integer> stack2=new LinkedList<>();
        int i=0;
        while(i<s.length()){
            if(Character.isDigit(arr[i])){
                int num=0;
                while(i<s.length()&&Character.isDigit(arr[i])){
                    num=num*10+arr[i]-'0';
                    i++;
                }
                stack2.push(num);
            }else if(arr[i]==']'){
                StringBuilder temp=new StringBuilder();
                int cnt=stack2.pop();
                while(stack1.peek()!='['){
                    temp.append(stack1.pop());
                }
                stack1.pop();
                for(int j=0;j<cnt;j++){
                    for(int k=temp.length()-1;k>=0;k--){
                        stack1.push(temp.charAt(k));
                    }
                }
                i++;
            }else{
                stack1.push(arr[i]);
                i++;
            }
        }
        while(!stack1.isEmpty()){
            ans.append(stack1.pop());
        }
        ans.reverse();
        return ans.toString();
    }
}
```
### 复杂度分析：
- 时间复杂度 $O(N)$，一次遍历 `s`；
- 空间复杂度 $O(N)$，辅助栈在极端情况下需要线性空间，例如 `2[2[2[a]]]`。


