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
自己的写法
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp=new int[amount+1];
        dp[0]=0;
        for(int i=1;i<=amount;i++){
            dp[i]=Integer.MAX_VALUE;
            boolean flag=false;
            for(int j=0;j<coins.length;j++){
                if(i-coins[j]>=0&&dp[i-coins[j]]!=-1){
                    dp[i]=Math.min(dp[i],dp[i-coins[j]]+1);
                    flag=true;
                }
            }
            if(flag==false){
                dp[i]=-1;
            }
        }
        return dp[amount];
    }
}
```
### 解法二:BFS
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
            int size=queue.size();
            boolean flag=false;
            for(int i=0;i<size;i++){
                int temp=queue.poll();
                for(int j=0;j<coins.length;j++){
                    int cur=temp-coins[j];
                    if(cur==0){
                        if(flag==false){
                            cnt++;
                            flag=true;
                        }
                        return cnt;
                    }
                    if(cur>0&&set.contains(cur)==false){
                        if(flag==false){
                            cnt++;
                            flag=true;
                        }
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
