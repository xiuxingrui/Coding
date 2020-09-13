# [LeetCode72.编辑距离](https://leetcode-cn.com/problems/edit-distance/)
## 题目描述
给你两个单词 `word1` 和 `word2`，请你计算出将 `word1` 转换成 `word2` 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

1. 插入一个字符
2. 删除一个字符
3. 替换一个字符

### 示例
```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```
```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```
## 题解
解释一：

`dp[i][j]` 代表 `word1` 到 `i` 位置转换成 `word2` 到 `j` 位置需要最少步数

所以，

- 当 `word1[i] == word2[j]`，`dp[i][j] = dp[i-1][j-1]`；
- 当 `word1[i] != word2[j]`，`dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1`

其中，`dp[i-1][j-1]` 表示替换操作，`dp[i-1][j]` 表示删除操作，`dp[i][j-1]` 表示插入操作。

注意，针对第一行，第一列要单独考虑，我们引入 '' 下图所示：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913175804.png)

第一行，是 `word1` 为空变成 `word2` 最少步数，就是插入操作

第一列，是 `word2` 为空，需要的最少步数，就是删除操作

解释二：

解决两个字符串的动态规划问题，一般都是用两个指针 `i`,`j` 分别指向两个字符串的最后，然后一步步往前走，缩小问题的规模。

设两个字符串分别为 `"rad"` 和 `"apple"`，为了把 `s1` 变成 `s2`，算法会这样进行：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913180838.gif)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913180850.png)

根据上面的 GIF，可以发现操作不只有三个，其实还有第四个操作，就是什么都不要做（skip）。比如这个情况：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913180914.png)

因为这两个字符本来就相同，为了使编辑距离最小，显然不应该对它们有任何操作，直接往前移动 `i`,`j` 即可。

还有一个很容易处理的情况，就是 `j` 走完 `s2` 时，如果 `i` 还没走完 `s1`，那么只能用删除操作把 `s1` 缩短为 `s2`。比如这个情况：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913181004.png)

类似的，如果 `i` 走完 `s1` 时 `j` 还没走完了 `s2`，那就只能用插入操作把 `s2` 剩下的字符全部插入 `s1`。等会会看到，这两种情况就是算法的 `base case`。

`base case` 是 `i` 走完 `s1` 或 `j` 走完 `s2`，可以直接返回另一个字符串剩下的长度。

对于每对儿字符 `s1[i]` 和 `s2[j]`，可以有四种操作：

```
if s1[i] == s2[j]:
    啥都别做（skip）
    i, j 同时向前移动
else:
    三选一：
        插入（insert）
        删除（delete）
        替换（replace）
```
有这个框架，问题就已经解决了。读者也许会问，这个「三选一」到底该怎么选择呢？很简单，全试一遍，哪个操作最后得到的编辑距离最小，就选谁。这里需要递归技巧，理解需要点技巧，先看下代码：

```
def minDistance(s1, s2) -> int:

    def dp(i, j):
        # base case
        if i == -1: return j + 1
        if j == -1: return i + 1
        
        if s1[i] == s2[j]:
            return dp(i - 1, j - 1)  # 啥都不做
        else:
            return min(
                dp(i, j - 1) + 1,    # 插入
                dp(i - 1, j) + 1,    # 删除
                dp(i - 1, j - 1) + 1 # 替换
            )
    
    # i，j 初始化指向最后一个索引
    return dp(len(s1) - 1, len(s2) - 1)
```

下面来详细解释一下这段递归代码，`base case` 应该不用解释了，主要解释一下递归部分。

都说递归代码的可解释性很好，这是有道理的，只要理解函数的定义，就能很清楚地理解算法的逻辑。我们这里 `dp(i, j)` 函数的定义是这样的：
```
def dp(i, j) -> int
# 返回 s1[0..i] 和 s2[0..j] 的最小编辑距离
```

记住这个定义之后，先来看这段代码：
```
if s1[i] == s2[j]:
    return dp(i - 1, j - 1)  # 啥都不做
# 解释：
# 本来就相等，不需要任何操作
# s1[0..i] 和 s2[0..j] 的最小编辑距离等于
# s1[0..i-1] 和 s2[0..j-1] 的最小编辑距离
# 也就是说 dp(i, j) 等于 dp(i-1, j-1)
```

如果 `s1[i]！=s2[j]`，就要对三个操作递归了，稍微需要点思考：

```
dp(i, j - 1) + 1,    # 插入
# 解释：
# 我直接在 s1[i] 插入一个和 s2[j] 一样的字符
# 那么 s2[j] 就被匹配了，前移 j，继续跟 i 对比
# 别忘了操作数加一
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913182241.gif)

```
dp(i - 1, j) + 1,    # 删除
# 解释：
# 我直接把 s[i] 这个字符删掉
# 前移 i，继续跟 j 对比
# 操作数加一
```

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913182318.gif)


```
dp(i - 1, j - 1) + 1 # 替换
# 解释：
# 我直接把 s1[i] 替换成 s2[j]，这样它俩就匹配了
# 同时前移 i，j 继续对比
# 操作数加一
```

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913182350.gif)

现在，你应该完全理解这段短小精悍的代码了。还有点小问题就是，这个解法是暴力解法，存在重叠子问题，需要用动态规划技巧来优化。

怎么能一眼看出存在重叠子问题呢？这里再简单提一下，需要抽象出本文算法的递归框架：

```
def dp(i, j):
    dp(i - 1, j - 1) #1
    dp(i, j - 1)     #2
    dp(i - 1, j)     #3
```

对于子问题 `dp(i-1, j-1)`，如何通过原问题 `dp(i, j)` 得到呢？有不止一条路径，比如 `dp(i, j) -> #1` 和 `dp(i, j) -> #2 -> #3`。一旦发现一条重复路径，就说明存在巨量重复路径，也就是重叠子
问题。

对于重叠子问题呢，优化方法无非是备忘录或者 `DP table`。

备忘录很好加，原来的代码稍加修改即可：

```
def minDistance(s1, s2) -> int:

    memo = dict() # 备忘录
    def dp(i, j):
        if (i, j) in memo: 
            return memo[(i, j)]
        ...
        
        if s1[i] == s2[j]:
            memo[(i, j)] = ...  
        else:
            memo[(i, j)] = ...
        return memo[(i, j)]
    
    return dp(len(s1) - 1, len(s2) - 1)
```

主要说下 `DP table` 的解法：

首先明确 `dp` 数组的含义，`dp` 数组是一个二维数组，长这样：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913182649.png)

有了之前递归解法的铺垫，应该很容易理解。`dp[..][0]` 和 `dp[0][..]` 对应 `base case`，`dp[i][j]` 的含义和之前的 `dp` 函数类似：

```
def dp(i, j) -> int
# 返回 s1[0..i] 和 s2[0..j] 的最小编辑距离

dp[i-1][j-1]
# 存储 s1[0..i] 和 s2[0..j] 的最小编辑距离
```

`dp` 函数的 `base case` 是 `i`,`j` 等于 -1，而数组索引至少是 0，所以 `dp` 数组会偏移一位。

既然 `dp` 数组和递归 `dp` 函数含义一样，也就可以直接套用之前的思路写代码，唯一不同的是，`DP table`是自底向上求解，递归解法是自顶向下求解：
```java
int minDistance(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    // base case 
    for (int i = 1; i <= m; i++)
        dp[i][0] = i;
    for (int j = 1; j <= n; j++)
        dp[0][j] = j;
    // 自底向上求解
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            if (s1.charAt(i-1) == s2.charAt(j-1))
                dp[i][j] = dp[i - 1][j - 1];
            else               
                dp[i][j] = min(
                    dp[i - 1][j] + 1,
                    dp[i][j - 1] + 1,
                    dp[i-1][j-1] + 1
                );
    // 储存着整个 s1 和 s2 的最小编辑距离
    return dp[m][n];
}

int min(int a, int b, int c) {
    return Math.min(a, Math.min(b, c));
}
```
一般来说，处理两个字符串的动态规划问题，都是按本文的思路处理，建立 `DP table`。为什么呢，因为易于找出状态转移的关系，比如编辑距离的 `DP table`：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913183347.png)

还有一个细节，既然每个 `dp[i][j]` 只和它附近的三个状态有关，空间复杂度是可以压缩成 $O(min(M,N))$ 的（`M`，`N` 是两个字符串的长度）。

只求出了最小的编辑距离，那具体的操作是什么?
```
// int[][] dp;
Node[][] dp;

class Node {
    int val;
    int choice;
    // 0 代表啥都不做
    // 1 代表插入
    // 2 代表删除
    // 3 代表替换
}
```

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1=word1.length(),len2=word2.length();
        char[] chs1=word1.toCharArray();
        char[] chs2=word2.toCharArray();
        int[][] dp=new int[len1+1][len2+1];
        for(int i=0;i<=len2;i++){
            dp[0][i]=i;
        }
        for(int i=0;i<=len1;i++){
            dp[i][0]=i;
        }
        for(int i=1;i<=len1;i++){
            for(int j=1;j<=len2;j++){
                if(chs1[i-1]==chs2[j-1]){
                    dp[i][j]=dp[i-1][j-1];
                }else{
                    dp[i][j]=Math.min(Math.min(dp[i][j-1],dp[i-1][j]),dp[i-1][j-1])+1;
                }
            }
        }
        return dp[len1][len2];
    }
}
```
空间优化：
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1=word1.length(),len2=word2.length();
        char[] chs1=word1.toCharArray();
        char[] chs2=word2.toCharArray();
        int[] dp=new int[len2+1];
        for(int i=0;i<=len2;i++){
            dp[i]=i;
        }
        for(int i=1;i<=len1;i++){
            int lastValue=dp[0];
            dp[0]++;
            for(int j=1;j<=len2;j++){
                int temp=dp[j];
                if(chs1[i-1]==chs2[j-1]){
                    dp[j]=lastValue;
                }else{
                    dp[j]=Math.min(Math.min(dp[j-1],dp[j]),lastValue)+1;
                }
                lastValue=temp;
            }
        }
        return dp[len2];
    }
}
```
### 复杂度分析
- 时间复杂度 ：$O(mn)$，其中 `m` 为 `word1` 的长度，`n` 为 `word2` 的长度。
- 空间复杂度 ：$O(mn)$，我们需要大小为 $O(mn)$ 的数组来记录状态值。
