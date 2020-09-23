# [LeetCode516.最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)
## 题目描述
给定一个字符串 `s` ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 `s` 的最大长度为 1000 。

- `1 <= s.length <= 1000`
- `s` 只包含小写英文字母
### 示例
```
"bbbab"
```
```
4
```
一个可能的最长回文子序列为 `"bbbb"`。
```
"cbbd"
```
```
2
```
一个可能的最长回文子序列为 `"bb"`。
## 题解
### 解法一
一般来说，这类问题都是让你求一个最长子序列，因为最短子序列就是一个字符嘛，没啥可问的。一旦涉及到子序列和最值，那几乎可以肯定，考察的是动态规划技巧，时间复杂度一般都是 $O(n^2)$。

1. 第一种思路模板是一个一维的 `dp` 数组：
```java
int n = array.length;
int[] dp = new int[n];

for (int i = 1; i < n; i++) {
    for (int j = 0; j < i; j++) {
        dp[i] = 最值(dp[i], dp[j] + ...)
    }
}
```
举个我们写过的例子「最长递增子序列」，在这个思路中 `dp` 数组的定义是：`

在子数组 `array[0..i]` 中，我们要求的子序列（最长递增子序列）的长度是 `dp[i]`。

2. 第二种思路模板是一个二维的 `dp` 数组：
```java
int n = arr.length;
int[][] dp = new dp[n][n];

for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        if (arr[i] == arr[j]) 
            dp[i][j] = dp[i][j] + ...
        else
            dp[i][j] = 最值(...)
    }
}
```
这种思路运用相对更多一些，尤其是涉及两个字符串/数组的子序列，比如前文讲的「最长公共子序列」。本思路中 `dp` 数组含义又分为「只涉及一个字符串」和「涉及两个字符串」两种情况。

涉及两个字符串/数组时（比如最长公共子序列），`dp` 数组的含义如下：

在子数组 `arr1[0..i]` 和子数组 `arr2[0..j]` 中，我们要求的子序列（最长公共子序列）长度为 `dp[i][j]`。

只涉及一个字符串/数组时（比如本文要讲的最长回文子序列），`dp` 数组的含义如下：

在子数组 `array[i..j]` 中，我们要求的子序列（最长回文子序列）的长度为 `dp[i][j]`。

之前解决了「最长回文子串」的问题，这次提升难度，求最长回文子序列的长度：

我们说这个问题对 `dp` 数组的定义是：在子串 `s[i..j]` 中，最长回文子序列的长度为 `dp[i][j]`。一定要记住这个定义才能理解算法。

如果我们想求 `dp[i][j]`，假设你知道了子问题 `dp[i+1][j-1]` 的结果（`s[i+1..j-1]` 中最长回文子序列的长度），你是否能想办法算出 `dp[i][j]` 的值（`s[i..j]` 中，最长回文子序列的长度）呢？

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924005819.png)

可以！这取决于 `s[i]` 和 `s[j]` 的字符：

如果它俩相等，那么它俩加上 `s[i+1..j-1]` 中的最长回文子序列就是 `s[i..j]` 的最长回文子序列：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924005943.png)

如果它俩不相等，说明它俩不可能同时出现在 `s[i..j]` 的最长回文子序列中，那么把它俩分别加入 `s[i+1..j-1]` 中，看看哪个子串产生的回文子序列更长即可：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924010013.png)

以上两种情况写成代码就是这样：

```java
if (s[i] == s[j])
    // 它俩一定在最长回文子序列中
    dp[i][j] = dp[i + 1][j - 1] + 2;
else
    // s[i+1..j] 和 s[i..j-1] 谁的回文子序列更长？
    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
```

至此，状态转移方程就写出来了，根据 `dp` 数组的定义，我们要求的就是 `dp[0][n - 1]`，也就是整个 `s` 的最长回文子序列的长度。

首先明确一下 `base case`，如果只有一个字符，显然最长回文子序列长度是 1，也就是 `dp[i][j] = 1 (i == j)`。

另外，看看刚才写的状态转移方程，想求 `dp[i][j]` 需要知道 `dp[i+1][j-1]`，`dp[i+1][j]`，`dp[i][j-1]` 这三个位置；再看看我们确定的 `base case`，填入 `dp` 数组之后是这样：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924010429.png)

为了保证每次计算 `dp[i][j]`，左下右方向的位置已经被计算出来，只能斜着遍历或者反着遍历：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924010504.png)

反着遍历：
```java
class Solution {
    int longestPalindromeSubseq(String s) {
        int n = s.length();
        // dp 数组全部初始化为 0
        int[][] dp=new int[n][n];
        // base case
        for (int i = 0; i < n; i++)
            dp[i][i] = 1;
        // 反着遍历保证正确的状态转移
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                // 状态转移方程
                if (s.charAt(i) == s.charAt(j)){
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                }
                else{
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        // 整个 s 的最长回文子串长度
        return dp[0][n - 1];
    }
}
```
另一种解释：

`dp[i][j]`表示字符串`s[i⋯j]s[i⋯j]`的最长回文子序列的长度。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924011757.png)

假设我们现在知道了`dp[i+1][j−1]`，我们如何计算`dp[i][j]`呢？我们只需要观察是`s[i]` 等不等于`s[j]`

- 当`s[i]=s[j]`，那么就说明在原先的基础上又增加了回文子序列的长度，`dp[i][j]=dp[i+1][j−1]+2`
- 当`s[i]≠s[j]`，那么那么说明`s[i]`、`s[j]`至少有一个不在回文子序列中。那就变成了下图所示：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924011934.png)

表明这时的`dp[i][j]`只需取两者之间的最大值即可。即`dp[i][j]=max(dp[i][j−1],dp[i+1][j])`。

由于我们的一个字符就能构成一个回文子序列，且长度为1，故`dp[i][j] = 1`，此时`i`一直是小于`j`的，不存在`i`大于`j`的情况。故我们的`dp`表为：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924012055.png)

由于这种初试值的方式，决定了我们遍历的顺序，我们需要需要从下向上的遍历，如图所示

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924012207.png)


![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924013018.gif)


```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        if(s == null){
            return 0;
        }
        int[][] dp = new int[s.length()][s.length()];
        int n = s.length();
        for(int i = 0; i<n; i++){
            dp[i][i] = 1;
        }
        //从下往上遍历
        for(int i = n-1; i>=0; i--){
            for(int j = i+1; j<n; j++){
                //那么就说明在原先的基础上又增加了回文子序列的长度
                if(s.charAt(i) == s.charAt(j)){
                    dp[i][j] = dp[i+1][j-1] + 2;
                }
                //表明这时dp[i][j]只需取两者之间的最大值即可
                else{
                    dp[i][j] = Math.max(dp[i+1][j],dp[i][j-1]);
                }
            }
        }
        return dp[0][n-1];

    }
}
```
### 解法二
自己的解法：
```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int len=s.length();
        if(len==0||len==1){
            return len;
        }
        char[] charArray=s.toCharArray();
        int[][] dp=new int[len][len];
        for(int i=0;i<len;i++){
            dp[i][i]=1;
            if(i<len-1){
                if(charArray[i]==charArray[i+1]){
                    dp[i][i+1]=2;
                }else{
                    dp[i][i+1]=1;
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
                    dp[i][j]=dp[i+1][j-1]+2;
                }else{
                    dp[i][j]=Math.max(dp[i+1][j],dp[i][j-1]);
                }
            }
        }
        return dp[0][len-1];
        
    }
}
```


