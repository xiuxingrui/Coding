# [LeetCode292.Nim游戏](https://leetcode-cn.com/problems/nim-game/)
## 题目描述
你和你的朋友，两个人一起玩 Nim 游戏：桌子上有一堆石头，每次你们轮流拿掉 1 - 3 块石头。 拿掉最后一块石头的人就是获胜者。你作为先手。

你们是聪明人，每一步都是最优解。 编写一个函数，来判断你是否可以在给定石头数量的情况下赢得游戏。

### 示例
```
输入: 4
输出: false 
解释: 如果堆中有 4 块石头，那么你永远不会赢得比赛；
     因为无论你拿走 1 块、2 块 还是 3 块石头，最后一块石头总是会被你的朋友拿走。
```
## 题解
### 解法一
面对4的整数倍的人永远无法翻身，你拿`N`根对手就会拿`4-N`根，保证每回合共减4根，你永远对面4倍数，直到4. 相反，如果最开始不是4倍数，你可以拿掉刚好剩下4倍数根，让他永远对面4倍数。(一个找规律的题，挺有意思的：当我拿完还剩1、2、3个时，必败，故我拿前有4个时必败，所以只要在我拿前有5、6、7个时，就可以必胜（5个时拿走一个，6拿2，7拿3，使对手转入拿前4个的必败状态），所以我拿前还有8个时必败（使对手转入必胜的拿前5、6、7状态）... ...)

让我们考虑一些小例子。显而易见的是，如果石头堆中只有一块、两块、或是三块石头，那么在你的回合，你就可以把全部石子拿走，从而在游戏中取胜。而如果就像题目描述那样，堆中恰好有四块石头，你就会失败。因为在这种情况下不管你取走多少石头，总会为你的对手留下几块，使得他可以在游戏中打败你。因此，要想获胜，在你的回合中，必须避免石头堆中的石子数为 4 的情况。

同样地，如果有五块、六块、或是七块石头，你可以控制自己拿取的石头数，总是恰好给你的对手留下四块石头，使他输掉这场比赛。但是如果石头堆里有八块石头，你就不可避免地会输掉，因为不管你从一堆石头中挑出一块、两块还是三块，你的对手都可以选择三块、两块或一块，以确保在再一次轮到你的时候，你会面对四块石头。

显然，它以相同的模式不断重复 `n=4,8,12,16,…`，基本可以看出是 4 的倍数。

#### 复杂度分析
- 时间复杂度：$O(1)$，只进行了一次检查。
- 空间复杂度：$O(1)$，没有使用额外的空间。
```java
class Solution {
    public boolean canWinNim(int n) {
        return n%4!=0;
    }
}
```
### 解法二
当前先手对应`n`个石头，后手就对应`n-1`、`n-2`、`n-3`三种集合的石头

每个集合一定都会对应必胜或者必败

因此可以得到这样一个递推关系：

只有当`n-1`、`n-2`、`n-3`三种集合都必胜时，`n`对应的集合才必败。因为不管`n`走哪条路，都一定对应着后手必胜，也就是对应着先手必败。

而但凡后手对应的`n-1`、`n-2`、`n-3`三种集合有一种对应的是必败，先手都一定是必胜。因为玩家是绝对聪明的，一定会走让后手必败的路线。

你拿走 1 个石子，然后不论对方从剩下的石子中拿走 1 个，2 个，还是 3 个，判断一下剩下的石子你是不是有稳赢的策略。

如果上边不行的话，你就拿走 2 个石子，然后再判断不论对方从剩下的石子拿走 1 个，2 个，还是3 个，剩下的石子你是不是都有稳赢的策略。

如果上边还不行的话，你就拿走 3 个石子，然后再判断不论对方从剩下的石子拿走 1 个，2 个，还是3 个，剩下的石子你是不是都有稳赢的策略。

刷题中常见的博弈问题，本质就是先手通过一系列操作，进来把当前状态变成对后手不利的状态。

由于选手足够聪明和规则巧妙设计，先手在给出的状态下总是必胜或者必败的。

必败态和必胜态的定义：
- 必胜态：如果一个状态的后继状态中存在必败态，那么这种该状态下，先手必胜。
- 必败态：如果一个状态的所有后继状态都是必胜态，那么这种状态下，先手必败。

不难发现这是一个递归的定义~

题目一定会给出一个最终状态(因为只有当游戏是收敛的，才有有结果)，选手无论如何抉择也一定会到达这个最终状态。比如本题中，把石子都拿走的状态。

这个最终状态要么为胜利，要么为失败。这个最终状态就是上面递归定义的终点~

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201011215946.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201011220211.png)

会超出内存限制：
```java
class Solution {
    public boolean canWinNim(int n) {
        if(n<4){
            return true;
        }
        boolean[] dp=new boolean[n+1];
        for(int i=1;i<=3;i++){
            dp[i]=true;
        }
        for(int i = 4; i <= n; ++i) {
            dp[i] = !dp[i-1] || !dp[i-2] || !dp[i-3];
        }
        return dp[n];
    }
}
```
或
```java
class Solution {
    public boolean canWinNim(int n) {
        if(n<4){
            return true;
        }
        boolean[][] dp=new boolean[n+1][2];
        for(int i=1;i<=3;i++){
            dp[i][0]=true;
            dp[i][1]=false;
        }
        for(int i=4;i<=n;i++){
            dp[i][0]=dp[i-1][1]||dp[i-2][1]||dp[i-3][1];
            dp[i][1]=dp[i-1][0]&&dp[i-2][0]&&dp[i-3][0];
        }
        return dp[n][0];
    }
}
```