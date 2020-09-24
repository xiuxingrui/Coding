# [LeetCode5.最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
## 题目描述
给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

### 示例
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```
```
输入: "cbbd"
输出: "bb"
```
## 题解
### 解法一:DP
对于一个子串而言，如果它是回文串，并且长度大于 2，那么将它首尾的两个字母去除之后，它仍然是个回文串。例如对于字符串 `“ababa”`，如果我们已经知道 `“bab”`是回文串，那么 `“ababa”`一定是回文串，这是因为它的首尾两个字母都是 `“a”`。

根据这样的思路，我们就可以用动态规划的方法解决本题。我们用 `P(i,j)` 表示字符串 `s` 的第 `i` 到 `j` 个字母组成的串（下文表示成 `s[i:j]`）是否为回文串：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200909145706.png)

这里的「其它情况」包含两种可能性：

- `s[i,j]` 本身不是一个回文串；
- `i>j`，此时 `s[i,j]`本身不合法。

那么我们就可以写出动态规划的状态转移方程：

`P(i,j)=P(i+1,j−1)&&(Si==Sj)`

也就是说，只有 `s[i+1:j−1]` 是回文串，并且 `s` 的第 `i` 和 `j` 个字母相同时，`s[i:j]` 才会是回文串。

上文的所有讨论是建立在子串长度大于 2 的前提之上的，我们还需要考虑动态规划中的边界条件，即子串的长度为 1 或 2。对于长度为 1 的子串，它显然是个回文串；对于长度为 2 的子串，只要它的两个字母相同，它就是一个回文串。因此我们就可以写出动态规划的边界条件：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200909145922.png)

根据这个思路，我们就可以完成动态规划了，最终的答案即为所有 `P(i,j)=true` 中子串长度的最大值。注意：在状态转移方程中，我们是从长度较短的字符串向长度较长的字符串进行转移的，因此一定要注意动态规划的循环顺序。

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s.length()==0){
            return "";
        }
        int n = s.length();
        char[] chs=s.toCharArray();
        boolean[][] dp = new boolean[n][n];
        int maxLen=1;
        int begin=0;
        for (int l = 1; l <=n; l++) {
            for (int i = 0; i + l-1< n; i++) {
                int j = i + l-1;
                if (l == 1) {
                    dp[i][j] = true;
                } else if (l == 2) {
                    dp[i][j] = (chs[i] == chs[j]);
                } else {
                    dp[i][j] = (chs[i] == chs[j] && dp[i + 1][j - 1]);
                }
                if (dp[i][j] && l > maxLen) {
                    maxLen=l;
                    begin=i;
                }
            }
        }
        return s.substring(begin,begin+maxLen);
    }
}
```
自己的写法：
```java
class Solution {
    public String longestPalindrome(String s) {
        int len=s.length();
        if(len==0){
            return "";
        }
        boolean[][] dp=new boolean[len][len];
        int maxLen=1;
        int begin=0;
        for(int i=0;i<len;i++){
            dp[i][i]=true;
            if(i<len-1){
                if(s.charAt(i)==s.charAt(i+1)){
                    dp[i][i+1]=true;
                    maxLen=2;
                    begin=i;
                }
            }
        }
        for(int l=3;l<=len;l++){//枚举长度
            for(int i=0;i<len;i++){
                int j=i+l-1;
                if(j>=len){
                    break;
                }
                if(s.charAt(i)==s.charAt(j)){
                    dp[i][j]=dp[i+1][j-1];
                    if(dp[i][j]==true){
                        maxLen=l;  
                        begin=i;
                    }
                }else{
                    dp[i][j]=false;
                }
            }
        }
        return s.substring(begin,begin+maxLen);
    }
}
```

由于`s.charAt(i)` 每次都会检查数组下标越界，因此先转换成字符数组
```java
class Solution {
    public String longestPalindrome(String s) {
        int len=s.length();
        if(len==0){
            return "";
        }
        boolean[][] dp=new boolean[len][len];
        int maxLen=1;
        int begin=0;
        char[] charArray = s.toCharArray();
        for(int i=0;i<len;i++){
            dp[i][i]=true;
            if(i<len-1){
                if(charArray[i]==charArray[i+1]){
                    dp[i][i+1]=true;
                    maxLen=2;
                    begin=i;
                }
            }
        }
        for(int l=3;l<=len;l++){
            for(int i=0;i<len;i++){
                int j=i+l-1;
                if(j>=len){
                    break;
                }
                if(charArray[i]==charArray[j]){
                    dp[i][j]=dp[i+1][j-1];
                    if(dp[i][j]==true){
                        maxLen=l;  
                        begin=i;
                    }
                }else{
                    dp[i][j]=false;
                }
            }
        }
        return s.substring(begin,begin+maxLen);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n^2)$，其中 `n` 是字符串的长度。动态规划的状态总数为 $O(n^2)$，对于每个状态，我们需要转移的时间为 $O(1)$。
- 空间复杂度：$O(n^2)$，即存储动态规划状态需要的空间。
### 解法二:暴力解

```java
public class Solution {

    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }

        int maxLen = 1;
        int begin = 0;
        // s.charAt(i) 每次都会检查数组下标越界，因此先转换成字符数组
        char[] charArray = s.toCharArray();

        // 枚举所有长度大于 1 的子串 charArray[i..j]
        for (int i = 0; i < len - 1; i++) {
            for (int j = i + 1; j < len; j++) {
                if (j - i + 1 > maxLen && validPalindromic(charArray, i, j)) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substring(begin, begin + maxLen);
    }

    /**
     * 验证子串 s[left..right] 是否为回文串
     */
    public boolean validPalindromic(char[] charArray, int left, int right) {
        while (left < right) {
            if (charArray[left] != charArray[right]) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N^3)$，这里 `N` 是字符串的长度，枚举字符串的左边界、右边界，然后继续验证子串是否是回文子串，这三种操作都与 `N` 相关；
- 空间复杂度：$O(1)$，只使用到常数个临时变量，与字符串长度无关。
