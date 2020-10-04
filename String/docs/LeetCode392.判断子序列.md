# [LeetCode392.判断子序列](https://leetcode-cn.com/problems/is-subsequence/)
## 题目描述
给定字符串 `s` 和 `t` ，判断 `s` 是否为 `t` 的子序列。

你可以认为 `s` 和 `t` 中仅包含英文小写字母。字符串 `t` 可能会很长（长度 ~= 500,000），而 `s` 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

如果有大量输入的 `S`，称作`S1, S2, ... , Sk` 其中 `k >= 10`亿，你需要依次检查它们是否为 `T` 的子序列。在这种情况下，你会怎样改变代码？
### 示例
```
s = "abc", t = "ahbgdc"

返回 true.

```
```
s = "axc", t = "ahbgdc"

返回 false.

```
## 题解
### 解法一
双指针匹配：
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if(s==null||s.length()==0){
            return true;
        }
        char[] arrS=s.toCharArray();
        char[] arrT=t.toCharArray();
        int i=0;
        for(char c:arrT){
            if(c==arrS[i]){
                i++;
                if(i==s.length()){
                    return true;
                }
            }
        }
        return false;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n+m)$，其中 `n` 为 `s` 的长度，`m` 为 `t` 的长度。每次无论是匹配成功还是失败，都有至少一个指针发生右移，两指针能够位移的总距离为 `n+m`。
- 空间复杂度：$O(1)$。
### 解法二
思路：

状态：`dp[i][j]`为`s`的从头开始到`i`的子字符串是否为`t`从头开始到`j`的子字符串的子序列

状态转移公式：

- 当`char[i]==char[j]`时，则字符`i`一定是`j`的子序列，如果`0~i-1`子字符串是`0~j-1`子字符串的子序列，则`dp[i][j]=true`，所以`dp[i][j] = dp[i-1][j-1]`；
- 当`char[i]!=char[j]`时，即判断当前`0~i`子字符串是否是`0~j-1`的子字符串的子序列，即`dp[i][j] = dp[i][j - 1]`。如`ab`，`eabc`，虽然`s`的最后一个字符和`t`中最后一个字符不相等，但是因为`ab`是`eab`的子序列，所以`ab`也是`eabc`的子序列

初始化：空字符串一定是`t`的子字符串的子序列，所以`dp[0][j]=true`

结果：返回`dp[sLen][tLen]`
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int sLen = s.length(), tLen = t.length();
        if (sLen > tLen) return false;
        if (sLen == 0) return true;
        boolean[][] dp = new boolean[sLen + 1][tLen + 1];
        //初始化
        for (int j = 0; j < tLen; j++) {
            dp[0][j] = true;
        }
        //dp
        for (int i = 1; i <= sLen; i++) {
            for (int j = 1; j <= tLen; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = dp[i][j - 1];
                }
            }
        }
        return dp[sLen][tLen];
    }
}
```s