# [LeetCode583.两个字符串的删除操作](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)
## 题目描述
给定两个单词 `word1` 和 `word2`，找到使得 `word1` 和 `word2` 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

- 给定单词的长度不超过500。
- 给定单词中的字符只含有小写字母。
### 示例
```
输入: "sea", "eat"
输出: 2
解释: 第一步将"sea"变为"ea"，第二步将"eat"变为"ea"
```
## 题解
我们使用二维数组 `dp`，现在 `dp[i][j]` 表示 `s1` 串前 `i` 个字符和 `s2` 串前 `j` 个字符匹配的最少删除次数。同样我们逐行求解 `dp` 数组。为了求出 `dp[i][j]` ，我们只考虑 2中情况：

- `s1[i−1]` 和 `s2[j−1]` 匹配，那么我们只需要让 `dp[i][j]` 赋值为 `dp[i−1][j−1]` 即可。这是因为两个字符串能匹配的字符不需要被删除。
- 字符 `s1[i−1]` 和 `s2[j−1]`不匹配。这种情况下，我们需要考虑删除两个字符中的哪一个，同时需要增加一次删除操作。两种可能的选择是 `dp[i−1][j]` 和 `dp[i][j−1]`。我们从中选择较小值。

最后，`dp[m][n]` 就是我们需要的最少删除次数，`m` 和 `n` 分别是字符串 `s1` 和字符串 `s2` 的长度。

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m=word1.length(),n=word2.length();
        char[] arr1=word1.toCharArray();
        char[] arr2=word2.toCharArray();
        int[][] dp=new int[m+1][n+1];
        dp[0][0]=0;
        for(int i=1;i<=m;i++){
            dp[i][0]=i;
        }
        for(int i=1;i<=n;i++){
            dp[0][i]=i;
        }
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(arr1[i-1]==arr2[j-1]){
                    dp[i][j]=dp[i-1][j-1];
                }else{
                    dp[i][j]=Math.min(dp[i-1][j],dp[i][j-1])+1;
                }
            }
        }
        return dp[m][n];
    }
}
```
### 复杂度分析
- 时间复杂度：$O(m*n)$。我们需要求出大小为 `m * n` 的 `dp` 数组，`m` 和 `n` 分别是两个字符串的长度。
- 空间复杂度：$O(m*n)$。使用了大小为 `m * n` 的 `dp` 数组。


