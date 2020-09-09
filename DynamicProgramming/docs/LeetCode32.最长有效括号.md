# [LeetCode32.最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)
## 题目描述
给定一个只包含 `'('` 和 `')'` 的字符串，找出最长的包含有效括号的子串的长度。

### 示例
```
输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
```
```
输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"
```
## 题解
我们定义 `dp[i]` 表示以下标 `i` 字符结尾的最长有效括号的长度。我们将 `dp` 数组全部初始化为 0 。显然有效的子串一定以 `‘)’` 结尾，因此我们可以知道以 `‘(’` 结尾的子串对应的 `dp` 值必定为 0，我们只需要求解 `‘)’` 在 `dp` 数组中对应位置的值。

我们从前往后遍历字符串求解 `dp` 值，我们每两个字符检查一次：

1. `s[i]=‘)’`且 `s[i−1]=‘(’`，也就是字符串形如 `“……()”`，我们可以推出：`dp[i]=dp[i−2]+2`

我们可以进行这样的转移，是因为结束部分的 `"()"` 是一个有效子字符串，并且将之前有效子字符串的长度增加了 2 。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200909232024.png)
2. `s[i]=‘)’` 且 `s[i−1]=‘)’`，也就是字符串形如 `“……))”`，我们可以推出：

如果 `s[i−dp[i−1]−1]=‘(’`，那么:`dp[i]=dp[i−1]+dp[i−dp[i−1]−2]+2`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200909232043.png)

我们考虑如果倒数第二个 `‘)’` 是一个有效子字符串的一部分（记作 $sub_{s}$），对于最后一个 `‘)’` ，如果它是一个更长子字符串的一部分，那么它一定有一个对应的 `‘(’` ，且它的位置在倒数第二个 `‘)’` 所在的有效子字符串的前面（也就是 $sub_{s}$ 的前面）。因此，如果子字符串 `subs`的前面恰好是 `‘(’`，那么我们就用 2 加上$sub_{s}$的长度（`dp[i−1]`）去更新 `dp[i]`。同时，我们也会把有效子串 “($sub_{s}$)”之前的有效子串的长度也加上，也就是再加上 `dp[i−dp[i−1]−2]`。

最后的答案即为 `dp` 数组中的最大值。

```java
class Solution {
    public int longestValidParentheses(String s) {
        int len=s.length();
        if(len==0||len==1){
            return 0;
        }
        int[] dp=new int[len];
        char[] charArray=s.toCharArray();
        int ans=0;
        for(int i=1;i<len;i++){
            if(charArray[i]==')'){
                if(charArray[i-1]=='('){
                    if(i>=2){
                        dp[i]=dp[i-2]+2;
                    }else{
                        dp[i]=2;
                    }
                    ans=ans>dp[i]?ans:dp[i];
                }else{
                    if(i-dp[i-1]>0&&charArray[i-dp[i-1]-1]=='('){
                        if(i-dp[i-1]-2>=0){
                            dp[i]=dp[i-dp[i-1]-2]+dp[i-1]+2;
                        }else{
                            dp[i]=dp[i-1]+2;
                        }
                        ans=ans>dp[i]?ans:dp[i];
                    }
                }
            }
        }
        return ans;
    }
}
```
### 复杂度分析
- 时间复杂度： $O(n)$，其中 `n` 为字符串的长度。我们只需遍历整个字符串一次，即可将 `dp` 数组求出来。
- 空间复杂度： $O(n)$。我们需要一个大小为 `n` 的 `dp` 数组。
