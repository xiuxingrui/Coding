# [LeetCode1143.最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
## 题目描述
给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长公共子序列的长度。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

若这两个字符串没有公共子序列，则返回 0。

- `1 <= text1.length <= 1000`
- `1 <= text2.length <= 1000`
- 输入的字符串只含有小写英文字符。

### 示例
```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
```
```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，它的长度为 3。
```
```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。
```
## 题解
我们暂时认为索引是从 1 开始的，待会的代码中只要稍作调整即可。其中，`dp[i][j]` 的含义是：对于 `s1[1..i]` 和 `s2[1..j]`，它们的 LCS 长度是 `dp[i][j]`。

我们专门让索引为 0 的行和列表示空串，`dp[0][..]` 和 `dp[..][0]` 都应该初始化为 0，这就是 base case。

- 当`s1[i]==s2[j]`,说明这两个字符是公共的字符，只要考察其子问题，`dp[i−1][j−1]`的长度即可，在此基础上+1,
- 当`s1[i]!=s2[j]`,说明这两个字符不是公共的字符，只要考察其两个子问题，`dp[i−1][j]`,`dp[i][j−1]`,取`max`

动态转移方程：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200908212257.png)



```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int len1=text1.length(),len2=text2.length();
        int[][] dp=new int[len1+1][len2+1];
        for(int i=1;i<len1+1;i++){
            for(int j=1;j<len2+1;j++){
                if(text1.charAt(i-1)==text2.charAt(j-1)){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=Math.max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[len1][len2];

    }
}
```


