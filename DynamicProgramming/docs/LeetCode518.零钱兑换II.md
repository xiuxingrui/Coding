# [LeetCode518.零钱兑换II](https://leetcode-cn.com/problems/coin-change-2/)
## 题目描述
给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。 

你可以假设：
- `0 <= amount (总金额) <= 5000`
- `1 <= coin (硬币面额) <= 5000`
- 硬币种类不超过 500 种
- 结果符合 32 位符号整数

### 示例
```
输入: amount = 5, coins = [1, 2, 5]
输出: 4
解释: 有四种方式可以凑成总金额:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```
```
输入: amount = 3, coins = [2]
输出: 0
解释: 只用面额2的硬币不能凑成总金额3。
```
```
输入: amount = 10, coins = [10] 
输出: 1
```
## 题解
### 解释一
在LeetCode上有两道题目非常类似，分别是

- 70.爬楼梯
- 518.零钱兑换 II

如果我们把每次可走步数/零钱面额限制为`[1,2]`, 把楼梯高度/总金额限制为3. 那么这两道题目就可以抽象成"给定`[1,2]`, 求组合成3的组合数和排列数"。

接下来引出本文的核心两段代码，虽然是Cpp写的，但是都是最基本的语法，对于可能看不懂的地方，我加了注释。

```c++
class Solution1 {
public:
    int change(int amount, vector<int>& coins) {
        int dp[amount+1];
        memset(dp, 0, sizeof(dp)); //初始化数组为0
        dp[0] = 1;
        for (int j = 1; j <= amount; j++){ //枚举金额
            for (int coin : coins){ //枚举硬币
                if (j < coin) continue; // coin不能大于amount
                dp[j] += dp[j-coin];
            }
        }
        return dp[amount];
    }
};
class Solution2 {
public:
    int change(int amount, vector<int>& coins) {
        int dp[amount+1];
        memset(dp, 0, sizeof(dp)); //初始化数组为0
        dp[0] = 1;
        for (int coin : coins){ //枚举硬币
            for (int j = 1; j <= amount; j++){ //枚举金额
                if (j < coin) continue; // coin不能大于amount
                dp[j] += dp[j-coin];
            }
        }
        return dp[amount];
    }
};
```
如果不仔细看，你会觉得这两个`Solution`似乎是一模一样的代码，但细心一点你会发现他们在嵌套循环上存在了差异。这个差异使得一个求解结果是排列数，一个求解结果是组合数。

在揭晓答案之前，让我们先分别用`DP`的方法解决爬楼梯和零钱兑换II的问题。每个解题步骤都按照`DP`三部曲，a.定义子问题，b. 定义状态数组，c. 定义状态转移方程。

爬楼梯：

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

这道题目子问题是，`problem(i) = sub(i-1) + sub(i-2)`, 即求解第i阶楼梯等于求解第`i-1`阶楼梯和第`i-2`阶楼梯之和。

状态数组是 `DP[i]`, 状态转移方程是`DP[i] = DP[i-1] = DP[i-2]`

```c++
class Solution {
public:
    int climbStairs(int n) {
        int DP[n+1];
        memset(DP, 0, sizeof(DP));
        DP[0] = 1;
        DP[1] = 1;
        for (int i = 2; i <= n; i++){
            DP[i] = DP[i-1] + DP[i-2] ;
        }
        return DP[n];

    }
};
```
由于每次我们只关注`DP[i-1]`和`DP[i-2]`，所以代码中能把数组替换成2个变量，降低空间复杂度，可以认为是将一维数组降维成点。

如果我们把问题泛化，不再是固定的1，2，而是任意给定台阶数，例如1,2,5呢？

我们只需要修改我们的`DP`方程`DP[i] = DP[i-1] + DP[i-2] + DP[i-5]`, 也就是`DP[i] = DP[i] + DP[i-j]` ,`j =1,2,5`

在原来的基础上，我们的代码可以做这样子修改

```c++
class Solution {
public:
    int climbStairs(int n) {
        int DP[n+1];
        memset(DP, 0, sizeof(DP));
        DP[0] = 1;
        int steps[2] = {1,2};
        for (int i = 1; i <= n; i++){
            for (int j = 0; j < 2; j++){
                int step = steps[j];
                if ( i < step ) continue;// 台阶少于跨越的步数
                DP[i] = DP[i] + DP[i-step];
            }
        }
        return DP[n];

    }
};
```
后续修改`steps`数组，就实现了原来问题的泛化。

那么这个代码是不是看起来很眼熟呢？我们能不能交换内外的循环呢？也就是下面的代码

```c++
for (int j = 0; j < 2; j++){
    int step = steps[j];
    for (int i = 1; i <= n; i++){
        if ( i < step ) continue;// 台阶少于跨越的步数
         DP[i] = DP[i] + DP[i-step];
    }
}
```
零钱兑换:

给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。

定义子问题: `problem(i) = sum( problem(i-j) ), j =1,2,5`. 含义为凑成总金额i的硬币组合数等于凑成总金额硬币`i-1, i-2, i-5,...`的子问题之和。

我们发现这个子问题定义居然和我们之前泛化的爬楼梯问题居然是一样的，那后面的状态数组和状态转移方程也是一样的，所以当前问题的代码可以在之前的泛化爬楼梯问题中进行修改而得。

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int dp[amount+1];
        memset(dp, 0, sizeof(dp)); //初始化数组为0
        dp[0] = 1;
        for (int j = 1; j <= amount; j++){ //枚举金额
            for (int i = 0; i < coins.size(): i++){ 
                int coin = coins[i]; //枚举硬币
                if (j < coin) continue; // coin不能大于amount
                dp[j] += dp[j-coin];
            }
        }
        return dp[amount];
    }
};
```
但是当你运行之后，却发现这个代码并不正确，得到的结果比预期的大。究其原因，该代码计算的结果是排列数，而不是组合数，也就是代码会把1,2和2,1当做两种情况。但更加根本的原因是我们子问题定义出现了错误。

正确的子问题定义应该是，`problem(k,i) = problem(k-1, i) + problem(k, i-k)`

即前`k`个硬币凑齐金额`i`的组合数等于前`k-1`个硬币凑齐金额`i`的组合数加上在原来`i-k`的基础上使用硬币的组合数。说的更加直白一点，那就是用前`k`的硬币凑齐金额`i`，要分为两种情况开率，一种是没有用前`k-1`个硬币就凑齐了，一种是前面已经凑到了`i-k`，现在就差第`k`个硬币了。

状态数组就是`DP[k][i]`, 即前`k`个硬币凑齐金额`i`的组合数。

这里不再是一维数组，而是二维数组。第一个维度用于记录当前组合有没有用到硬币`k`，第二个维度记录现在凑的金额是多少？如果没有第一个维度信息，当我们凑到金额`i`的时候，我们不知道之前有没有用到硬币`k`。

因为这是个组合问题，我们不关心硬币使用的顺序，而是硬币有没有被用到。是否使用第`k`个硬币受到之前情况的影响。

状态转移方程如下

```
if 金额数大于硬币
    DP[k][i] = DP[k-1][i] + DP[k][i-k]
else
    DP[k][i] = DP[k-1][i]
```

因此正确代码如下:

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int K = coins.size() + 1;
        int I = amount + 1;
        int DP[K][I];
        //初始化数组
        for (int k = 0; k < K; k++){
            for (int i = 0; i < I; i++){
                DP[k][i] = 0;
            }
        }
        //初始化基本状态
        for (int k = 0; k < coins.size() + 1; k++){
            DP[k][0] = 1;
        }
        for (int k = 1; k <= coins.size() ; k++){
            for (int i = 1; i <= amount; i++){  
                if ( i >= coins[k-1]) {
                    DP[k][i] = DP[k][i-coins[k-1]] + DP[k-1][i]; 
                } else{
                    DP[k][i] = DP[k-1][k];
                }
            }
        }
        return DP[coins.size()][amount];
    }
};
```
我们初始化的数组大小为`coins.size()+1* (amount+1)`, 这是因为第一列是硬币为0的基本情况。

此时，交换这里面的循环不会影响最终的结果。也就是
```c++
for (int i = 1; i <= amount; i++){  
    for (int k = 1; k <= coins.size() ; k++){ 
        if ( i >= coins[k-1]) {
            DP[k][i] = DP[k][i-coins[k-1]] + DP[k-1][i]; 
         } else{
             DP[k][i] = DP[k-1][k];
         }
     }
}
```
之前爬楼梯问题中，我们将一维数组降维成点。这里问题能不能也试着降低一个维度，只用一个数组进行表示呢？

这个时候，我们就需要重新定义我们的子问题了。

此时的子问题是，对于硬币从0到`k`，我们必须使用第`k`个硬币的时候，当前金额的组合数。

因此状态数组`DP[i]`表示的是对于第`k`个硬币能凑的组合数

状态转移方程如下
```
DP[[i] = DP[i] + DP[i-k]
```
于是得到我们开头的第二个Solution。
```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int dp[amount+1];
        memset(dp, 0, sizeof(dp)); //初始化数组为0
        dp[0] = 1;
        for (int coin : coins){ //枚举硬币
            for (int i = 1; i <= amount; i++){ //枚举金额
                if (i < coin) continue; // coin不能大于amount
                dp[i] += dp[i-coin];
            }
        }
        return dp[amount];
    }
};
```
好了，继续之前的问题，这里的内外循环能换吗？

显然不能，因为我们这里定义的子问题是，必须选择第`k`个硬币时，凑成金额`i`的方案。如果交换了，我们的子问题就变了，那就是对于金额`i`, 我们选择硬币的方案。

同样的，我们回答之前爬楼梯的留下的问题，原循环结构对应的子问题是，对于楼梯数i, 我们的爬楼梯方案。第二种循环结构则是，固定爬楼梯的顺序，我们爬楼梯的方案。也就是第一种循环下，对于楼梯3，你可以先2再1，或者先1再2，但是对于第二种循环,1,2是一种情况。

看完文章后顿悟组合问题其实是在每一次for coin in coins循环中就把coin的可使用次数规定好了。不允许在后面的硬币层次使用之前的硬币。 这就像排列中2,2,1; 2,1,2是两种情况，但是组合问题规定好了一种书写顺序，比如大的写在前面那就只有2,2,1这一种情况了。

### 解释二
这道题可以套用「完全背包」的模型，不过我们这里和大家推导一下公式，以加深对「完全背包」模型的理解。

- 「完全背包」问题的特点是：背包里的物品可以无限次选取；
- 本题特殊的地方在于：从背包里选取的物品 必须刚好装满 需要考虑的容量，而不是小于等于，注意这点细微的区别。

「完全背包」问题是「0-1」背包问题的扩展。它们的区别在于：

- 「0-1」背包问题：当前考虑的物品拿或者不拿；
- 「完全」背包问题：当前考虑的物品拿或者不拿，如果拿，只要背包能装下，就可以一直拿，直到背包装不下为止。

思考状态和状态转移方程：

第 1 步：定义状态

`dp[i][j]`：硬币列表的前缀子区间 `[0, i]` 能够凑成总金额为 `j` 的组合数。

说明：背包问题有一个特点，顺序无关，在本题解的最开始，我们强调过这道问题的这个性质，因此可以**一个一个硬币**去考虑。

第 2 步：状态转移方程（基础状态转移方程，本题解的后半部分有优化）

对于遍历到的每一种面值的硬币，逐个考虑添加到 「总金额」 中。由于硬币的个数可以无限选取，因此对于一种新的面值的硬币 `coins[i]`，依次考虑选取 0 枚、1 枚、2 枚，以此类推，直到选取这种面值的硬币的总金额超过需要的总金额 `j` 为止。

```java
dp[i][j] = dp[i - 1][j - 0 * coins[i]] + 
           dp[i - 1][j - 1 * coins[i]] +
           dp[i - 1][j - 2 * coins[i]] + 
           ... + 
           dp[i - 1][j - k * coins[i]]
```

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200906165103.png)

上述公式需要满足：`j - k * coins[i] >= 0`。`dp[i][j]` 相对于 `dp[i - 1][j]` 而言，多考虑的一枚硬币，是当前正在考虑的那枚硬币的面值，`coins[i]`，而这枚硬币选取的个数（从 0 开始）就是 `dp[i][j]` 这个问题可以分解的各个子问题的分类标准。

第 3 步：思考初始化

`dp[0][0]` 的值设置为 1，这一点可能比较难理解，但它作为被参考的值，可以使得后续的状态转移方程正确。原因是：当 `dp[i - 1][j - k * coins[i]]` 的第 2 维 `j - k * coins[i] == 0` 成立的时候，`k` 个硬币 `coin[i]` 恰好成为了一种组合。因此，`dp[0][0] = 1` 是合理的（代入状态转移方程，值正确）。

### 解释三
第一步要明确两点，「状态」和「选择」。

状态有两个，就是「背包的容量」和「可选择的物品」，选择就是「装进背包」或者「不装进背包」嘛，背包问题的套路都是这样。

明白了状态和选择，动态规划问题基本上就解决了，只要往这个框架套就完事儿了：

```
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 计算(选择1，选择2...)
```

第二步要明确 `dp` 数组的定义。

首先看看刚才找到的「状态」，有两个，也就是说我们需要一个二维 `dp` 数组。

`dp[i][j]` 的定义如下：

若只使用前 `i` 个物品，当背包容量为 `j` 时，有 `dp[i][j]` 种方法可以装满背包。

若只使用 `coins` 中的前 `i` 个硬币的面值，若想凑出金额 `j`，有 `dp[i][j]` 种凑法。

经过以上的定义，可以得到：

`base case` 为 `dp[0][..] = 0`， `dp[..][0] = 1`。因为如果不使用任何硬币面值，就无法凑出任何金额；如果凑出的目标金额为 0，那么“无为而治”就是唯一的一种凑法。

我们最终想得到的答案就是 `dp[N][amount]`，其中 `N` 为 `coins` 数组的大小。

第三步，根据「选择」，思考状态转移的逻辑。

注意，我们这个问题的特殊点在于物品的数量是无限的，所以这里和之前写的背包问题文章有所不同。

如果你不把这第 `i` 个物品装入背包，也就是说你不使用 `coins[i]` 这个面值的硬币，那么凑出面额 `j` 的方法数 `dp[i][j]` 应该等于 `dp[i-1][j]`，继承之前的结果。

如果你把这第 `i` 个物品装入了背包，也就是说你使用 `coins[i]` 这个面值的硬币，那么 `dp[i][j]` 应该等于 `dp[i][j-coins[i-1]]`。

首先由于 `i` 是从 1 开始的，所以 `coins` 的索引是 `i-1` 时表示第 `i` 个硬币的面值。

`dp[i][j-coins[i-1]]` 也不难理解，如果你决定使用这个面值的硬币，那么就应该关注如何凑出金额 `j - coins[i-1]`。

综上就是两种选择，而我们想求的 `dp[i][j]` 是「共有多少种凑法」，所以 `dp[i][j]` 的值应该是以上两种选择的结果之和：

```java
int change(int amount, int[] coins) {
    int n = coins.length;
    int[][] dp = amount int[n + 1][amount + 1];
    // base case
    for (int i = 0; i <= n; i++) 
        dp[i][0] = 1;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= amount; j++)
            if (j - coins[i-1] >= 0)
                dp[i][j] = dp[i - 1][j] 
                         + dp[i][j - coins[i-1]];
            else 
                dp[i][j] = dp[i - 1][j];
    }
    return dp[n][amount];
}
```

```java
class Solution {
    public int change(int amount, int[] coins) {
        int len=coins.length;
        int[][] dp=new int[len+1][amount+1];
        for(int i=0;i<len+1;i++){
            dp[i][0]=1;
        }
        for(int i=1;i<len+1;i++){
            for(int j=1;j<amount+1;j++){
                dp[i][j]=dp[i-1][j];
                if(coins[i-1]<=j){
                    dp[i][j]=dp[i][j]+dp[i][j-coins[i-1]];
                }
            }
        }
        return dp[len][amount];
    }
}
```
优化空间：
```java
class Solution {
  public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;

    for (int coin : coins) {
      for (int x = coin; x < amount + 1; ++x) {
        dp[x] += dp[x - coin];
      }
    }
    return dp[amount];
  }
}
```
### 复杂度分析
- 时间复杂度：$O(N×amount)$。其中 `N` 为 `coins` 数组的长度。
- 空间复杂度：$O(amount)$，`dp` 数组使用的空间。