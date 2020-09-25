# [LeetCode91.解码方法](https://leetcode-cn.com/problems/decode-ways/)
## 题目描述
一条包含字母 `A-Z` 的消息通过以下方式进行了编码：
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
给定一个只包含数字的非空字符串，请计算解码方法的总数。
### 示例
```
输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
```
```
输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。
```
## 题解
```java
class Solution {
    public int numDecodings(String s) {
        int len=s.length();
        int[] dp=new int[len+1];
        dp[0]=1;
        if(s.charAt(0)!='0'){
            dp[1]=1;
        }
        for(int i=2;i<len+1;i++){
            if(s.charAt(i-1)!='0'){
                dp[i]=dp[i-1];
            }
            int currentNum=(s.charAt(i-2)-'0')*10+s.charAt(i-1)-'0';
            if(currentNum>9&&currentNum<27){
                dp[i]+=dp[i-2];
            }
        }
        return dp[len];
    }
}
```
