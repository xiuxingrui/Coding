# [LeetCode459.重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/)
## 题目描述
给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。

### 示例
```
输入: "abab"

输出: True

解释: 可由子字符串 "ab" 重复两次构成。
```
```
输入: "aba"

输出: False
```
```
输入: "abcabcabcabc"

输出: True

解释: 可由子字符串 "abc" 重复四次构成。 (或者子字符串 "abcabc" 重复两次构成。)
```
## 题解
### 解法一
当 `s` 没有循环节时：

如果 `s` 中没有循环节，那么 `s` 中必然有且只有两个 `s`，此时从 `s[1]` 处开始寻找 `s` ，必然只能找到第二个。所以此时返回值为 `s.length()`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201003210709.png)

当 `s` 有循环节时：

当 `s` 中有循环节时，设循环节为 `r`，其长度为 `l`，那么 `s` 中必然有 `s.length()/l + 1` 个 `s`。

因为去掉了第一个 `S` 的第一个字符 (代码中，`(s+s).find(s, 1)`， 是从 `s[1]` 处开始 `find` )

所以此时必回找到第二个 `s` 的起点。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201003210929.png)

在下面的代码中，我们可以从位置 1 开始查询，并希望查询结果不为位置 `n`，这与移除字符串的第一个和最后一个字符是等价的。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).indexOf(s, 1) != s.length();
    }
}
```
移除首尾：
```java
class Solution {
   public boolean repeatedSubstringPattern(String s) {
        String str = s + s;
        return str.substring(1, str.length() - 1).contains(s);
    }
}
```
### 解法二
枚举:

如果一个长度为 `n` 的字符串 `s` 可以由它的一个长度为 `n'`的子串 `s'`重复多次构成，那么：

- `n` 一定是 `n'`的倍数；
- `s'`′一定是 `s` 的前缀；
- 对于任意的  `i∈[n',n)`，有 `s[i]=s[i−n’]`。

也就是说，`s` 中长度为 `n'`的前缀就是 `s'`，并且在这之后的每一个位置上的字符 `s[i]`，都需要与它之前的第 `n'`个字符 `s[i-n']`相同。

因此，我们可以从小到大枚举 `n'`，并对字符串 `s` 进行遍历，进行上述的判断。注意到一个小优化是，因为子串至少需要重复一次，所以 `n'`不会大于 `n` 的一半，我们只需要在 `[1,n/2]`的范围内枚举 `n'`即可。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        for (int i = 1; i * 2 <= n; ++i) {
            if (n % i == 0) {
                boolean match = true;
                for (int j = i; j < n; ++j) {
                    if (s.charAt(j) != s.charAt(j - i)) {
                        match = false;
                        break;
                    }
                }
                if (match) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(n^2)$，其中 `n` 是字符串 `s` 的长度。枚举 `i` 的时间复杂度为 $O(n)$，遍历 `s` 的时间复杂度为 $O(n)$，相乘即为总时间复杂度。
- 空间复杂度：$O(1)$。
### 解法三
`KMP`算法

1. 设查询串的的长度为 `n`，模式串的长度为 `m`，我们需要判断模式串是否为查询串的子串。那么使用 `KMP` 算法处理该问题时的时间复杂度是多少？在分析时间复杂度时使用了哪一种分析方法？

- 时间复杂度为 $O(n+m)$，用到了均摊分析（摊还分析）的方法。
- 具体地，无论在预处理过程还是查询过程中，虽然匹配失败时，指针会不断地根据 `fail` 数组向左回退，看似时间复杂度会很高。但考虑匹配成功时，指针会向右移动一个位置，这一部分对应的时间复杂度为 $O(n+m)$。又因为向左移动的次数不会超过向右移动的次数，因此总时间复杂度仍然为 $O(n+m)$。

2. 如果有多个查询串，平均长度为 `n`，数量为 `k`，那么总时间复杂度是多少？

时间复杂度为 $O(nk+m)$。模式串只需要预处理一次。

3. 在 `KMP` 算法中，对于模式串，我们需要预处理出一个 `fail` 数组（有时也称为 `next` 数组）。这个数组到底表示了什么？

`fail[i]` 等于满足下述要求的 `x` 的最大值：`s[0:i]` 具有长度为 `x+1` 的完全相同的前缀和后缀。这也是 `KMP` 算法最重要的一部分。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return kmp(s + s, s);
    }

    public boolean kmp(String query, String pattern) {
        int n = query.length();
        int m = pattern.length();
        int[] fail = new int[m];
        Arrays.fill(fail, -1);
        for (int i = 1; i < m; ++i) {
            int j = fail[i - 1];
            while (j != -1 && pattern.charAt(j + 1) != pattern.charAt(i)) {
                j = fail[j];
            }
            if (pattern.charAt(j + 1) == pattern.charAt(i)) {
                fail[i] = j + 1;
            }
        }
        int match = -1;
        for (int i = 1; i < n - 1; ++i) {
            while (match != -1 && pattern.charAt(match + 1) != query.charAt(i)) {
                match = fail[match];
            }
            if (pattern.charAt(match + 1) == query.charAt(i)) {
                ++match;
                if (match == m - 1) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是字符串 `s` 的长度。
- 空间复杂度：$O(n)$。
