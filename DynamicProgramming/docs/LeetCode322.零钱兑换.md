# [LeetCode322.零钱兑换](https://leetcode-cn.com/problems/coin-change/)
## 题目描述
给定不同面额的硬币 `coins` 和一个总金额 `amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

你可以认为每种硬币的数量是无限的。
### 示例
```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```
```
输入: coins = [2], amount = 3
输出: -1
```
## 题解
### 解法一:动态规划
自下而上:

我们采用自下而上的方式进行思考。仍定义 `F(i)` 为组成金额 `i` 所需最少的硬币数量，假设在计算 `F(i)` 之前，我们已经计算出 `F(0)~F(i−1)`的答案。 则 `F(i)` 对应的转移方程应为

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200826183315.png)

其中$c_{j}$代表的是第 `j`枚硬币的面值，即我们枚举最后一枚硬币面额是$c_{j}$,那么需要从$i-c_{j}$这个金额的状态$F\left(i-c_{j}\right)$转移过来，再算上枚举的这枚硬币数量 1 的贡献，由于要硬币数量最少，所以 `F(i)` 为前面能转移过来的状态的最小值加上枚举的硬币数量 1 。

例子1：假设

```java
coins = [1, 2, 5], amount = 11
```
则，当 `i==0` 时无法用硬币组成，为 0 。当 `i<0` 时，忽略 `F(i)`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200826183825.png)

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp=new int[amount+1];
        //Arrays.fill(dp, amount + 1);
        dp[0]=0;
        for(int i=1;i<=amount;i++){
            dp[i]=amount+1;
            for(int j=0;j<coins.length;j++){
                if(i-coins[j]>=0){
                    dp[i]=Math.min(dp[i],dp[i-coins[j]]+1);
                }
            }
        }
        return dp[amount]>amount?-1:dp[amount];
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(Sn)$，其中 `S` 是金额，`n` 是面额数。我们一共需要计算 $O(S)$ 个状态，`S` 为题目所给的总金额。对于每个状态，每次需要枚举 `n` 个面额来转移状态，所以一共需要 $O(Sn)$ 的时间复杂度。
- 空间复杂度：$O(S)$。`DP`数组需要开长度为总金额 `S` 的空间。
### 解法二:DFS
`DFS` 搜索顺序为面值从大到小，同一面值数量由多到少，搜索过程中记录到此为止所花费的硬币数。

剪枝处理如下，具体位置见代码注释：

- 当 `amount==0`时剪枝，因为大面值硬币需要更多小面值硬币替换，继续减少一枚或几枚大硬币搜索出来的答案【如果有】肯定不会更优。
- 当 `amount!=0`，但已经使用的硬币数 `cnt` 满足了 `cnt+1>=ans`时剪枝，因 `amount` 不为 0，至少还要使用 1 枚硬币，则继续搜索得到的答案不会更优。是 `break` 不是 `continue`，因为少用几枚面值大的硬币搜索出的方案【如果有】肯定需要更多枚面值小的硬币来替换掉这枚大的硬币【同剪枝 1 理由】，而使用这几枚大的硬币都搜索不到更好的解，故应该是 `break`。

```java
class Solution {
    int ans=Integer.MAX_VALUE;
    public int coinChange(int[] coins, int amount) {
        Arrays.sort(coins);
        dfs(coins,coins.length-1,amount,0);
        return ans==Integer.MAX_VALUE?-1:ans;
    }
    public void dfs(int[] coins,int index,int amount,int cnt){
        if(index<0){
            return;
        }
        for(int c=amount/coins[index];c>=0;c--){
            int na=amount-c*coins[index];
            int ncnt=cnt+c;
            if(na==0){
                ans=Math.min(ans,ncnt);
                break;//剪枝1
            }
            if(ncnt+1>=ans){
                break; //剪枝2
            }
            dfs(coins,index-1,na,ncnt);
        }
    }
}
```
### 解法三:BFS
自己的写法:
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(amount==0){
            return 0;
        }
        Queue<Integer> queue=new LinkedList<>();
        HashSet<Integer> set=new HashSet<>();
        queue.offer(amount);
        int cnt=0;
        while(!queue.isEmpty()){
            cnt++;
            int size=queue.size();
            boolean flag=false;
            for(int i=0;i<size;i++){
                int temp=queue.poll();
                for(int j=0;j<coins.length;j++){
                    int cur=temp-coins[j];
                    if(cur==0){
                        return cnt;
                    }
                    if(cur>0&&set.contains(cur)==false){
                        set.add(cur);
                        queue.offer(cur);
                    }
                }
            }
        }
        return -1;
    }
}
```
### 解法三:DFS
