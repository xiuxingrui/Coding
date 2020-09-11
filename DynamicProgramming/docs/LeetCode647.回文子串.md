# [LeetCode647.回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)
## 题解
给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

输入的字符串长度不会超过 1000 。
### 示例
```
输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```
```
输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```
## 题解
```java
class Solution {
    public int countSubstrings(String s) {
        int len=s.length();
        if(len==0){
            return 0;
        }
        boolean[][] dp=new boolean[len][len];
        char[] charArray=s.toCharArray();
        int ans=len;
        for(int i=0;i<len;i++){
            dp[i][i]=true;
            if(i<len-1){
                if(charArray[i]==charArray[i+1]){
                    dp[i][i+1]=true;
                    ans++;
                }
            }
        }
        for(int l=3;l<=len;l++){
            for(int i=0;i+l-1<len;i++){
                int j=i+l-1;
                if(charArray[i]==charArray[j]&&dp[i+1][j-1]==true){
                    dp[i][j]=true;
                    ans++;
                }
            }
        }
        return ans;
    }
}
```