# [LeetCode174.地下城游戏](https://leetcode-cn.com/problems/dungeon-game/)
## 题目描述
一些恶魔抓住了公主（`P`）并将她关在了地下城的右下角。地下城是由 `M x N` 个房间组成的二维网格。我们英勇的骑士（`K`）最初被安置在左上角的房间里，他必须穿过地下城并通过对抗恶魔来拯救公主。

骑士的初始健康点数为一个正整数。如果他的健康点数在某一时刻降至 0 或以下，他会立即死亡。

有些房间由恶魔守卫，因此骑士在进入这些房间时会失去健康点数（若房间里的值为负整数，则表示骑士将损失健康点数）；其他房间要么是空的（房间里的值为 0），要么包含增加骑士健康点数的魔法球（若房间里的值为正整数，则表示骑士将增加健康点数）。

为了尽快到达公主，骑士决定每次只向右或向下移动一步。

编写一个函数来计算确保骑士能够拯救到公主所需的最低初始健康点数。

例如，考虑到如下布局的地下城，如果骑士遵循最佳路径 右 -> 右 -> 下 -> 下，则骑士的初始健康点数至少为 7。

```
-2 (K)	-3	  3
-5	   -10	  1
10	    30	-5 (P)
```
- 骑士的健康点数没有上限。
- 任何房间都可能对骑士的健康点数造成威胁，也可能增加骑士的健康点数，包括骑士进入的左上角房间以及公主被监禁的右下角房间。

## 题解
### 解法一
几个要素：「`M×N` 的网格」「每次只能向右或者向下移动一步」。让人很容易想到该题使用动态规划的方法。

但是我们发现，如果按照从左上往右下的顺序进行动态规划，对于每一条路径，我们需要同时记录两个值。第一个是「从出发点到当前点的路径和」，第二个是「从出发点到当前点所需的最小初始值」。而这两个值的重要程度相同，参看下面的示例：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028144314.png)

从 (0,0) 到 (1,2) 有多条路径，我们取其中最有代表性的两条：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028144336.png)

绿色路径「从出发点到当前点的路径和」为 1，「从出发点到当前点所需的最小初始值」为 3。

蓝色路径「从出发点到当前点的路径和」为 −1，「从出发点到当前点所需的最小初始值」为 2。

我们希望「从出发点到当前点的路径和」尽可能大，而「从出发点到当前点所需的最小初始值」尽可能小。这两条路径各有优劣。

在上图中，我们知道应该选取绿色路径，因为蓝色路径的路径和太小，使得蓝色路径需要增大初始值到 4 才能走到终点，而绿色路径只要 3 点初始值就可以直接走到终点。但是如果把终点的 −2 换为 0，蓝色路径只需要初始值 2，绿色路径仍然需要初始值 3，最优决策就变成蓝色路径了。

因此，如果按照从左上往右下的顺序进行动态规划，我们无法直接确定到达 `(1,2)` 的方案，因为有两个重要程度相同的参数同时影响后续的决策。也就是说，这样的动态规划是不满足「无后效性」的。

于是我们考虑从右下往左上进行动态规划。令 `dp[i][j]` 表示从坐标 `(i,j)` 到终点所需的最小初始值。换句话说，当我们到达坐标 `(i,j)` 时，如果此时我们的路径和不小于 `dp[i][j]`，我们就能到达终点。

这样一来，我们就无需担心路径和的问题，只需要关注最小初始值。对于 `dp[i][j]`，我们只要关心 `dp[i][j+1]` 和 `dp[i+1][j]` 的最小值 `minn`。记当前格子的值为 `dungeon(i,j)`，那么在坐标 `(i,j)` 的初始值只要达到 `minn−dungeon(i,j)` 即可。同时，初始值还必须大于等于 111。这样我们就可以得到状态转移方程：

`dp[i][j]=max(min(dp[i+1][j],dp[i][j+1])−dungeon(i,j),1)`

最终答案即为 `dp[0][0]`。

边界条件为，当 `i=n−1` 或者 `j=m−1` 时，`dp[i][j]`转移需要用到的 `dp[i][j+1]` 和 `dp[i+1][j]` 中有无效值，因此代码实现中给无效值赋值为极大值。特别地，`dp[n−1][m−1]` 转移需要用到的 `dp[n−1][m]` 和 `dp[n][m−1]` 均为无效值，因此我们给这两个值赋值为 1。
写法一
```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int n = dungeon.length, m = dungeon[0].length;
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 0; i <= n; ++i) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }
        dp[n][m - 1] = dp[n - 1][m] = 1;
        for (int i = n - 1; i >= 0; --i) {
            for (int j = m - 1; j >= 0; --j) {
                int minn = Math.min(dp[i + 1][j], dp[i][j + 1]);
                dp[i][j] = Math.max(minn - dungeon[i][j], 1);
            }
        }
        return dp[0][0];
    }
}
```
写法二：
```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if(dungeon==null||dungeon.length==0||dungeon[0].length==0){
            return 0;
        }
        int m=dungeon.length,n=dungeon[0].length;
        int[][] dp=new int[m][n];
        dp[m-1][n-1]=dungeon[m-1][n-1]>=0?1:-dungeon[m-1][n-1]+1;
        for(int i=m-2;i>=0;i--){
            dp[i][n-1]=dp[i+1][n-1]-dungeon[i][n-1]>0?dp[i+1][n-1]-dungeon[i][n-1]:1;
        }
        for(int j=n-2;j>=0;j--){
            dp[m-1][j]=dp[m-1][j+1]-dungeon[m-1][j]>0?dp[m-1][j+1]-dungeon[m-1][j]:1;
        }
        
        for(int i=m-2;i>=0;i--){
            for(int j=n-2;j>=0;j--){
                int needMin=Math.min(dp[i][j+1],dp[i+1][j])-dungeon[i][j];
                dp[i][j]=needMin>0?needMin:1;
            }
        }
        return dp[0][0];
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N×M)$，其中 `N`,`M` 为给定矩阵的长宽。
- 空间复杂度：$O(N×M)$，其中 `N`,`M` 为给定矩阵的长宽，注意这里可以利用滚动数组进行优化，优化后空间复杂度可以达到 $O(N)$。
### 解法二
记忆化：
```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if(dungeon==null||dungeon.length==0||dungeon[0].length==0){
            return 0;
        }
        int[][] memo=new int[dungeon.length][dungeon[0].length];
        for(int i=0;i<dungeon.length;i++){
            for(int j=0;j<dungeon[0].length;j++){
                memo[i][j]=-1;
            }
        }
        return dfs(dungeon,0,0,memo);
    }
    public int dfs(int[][] dungeon,int i,int j,int[][] memo){
        if(i>=dungeon.length||j>=dungeon[0].length){
            return Integer.MAX_VALUE;
        }
        if(memo[i][j]!=-1){
            return memo[i][j];
        }
        if(i==dungeon.length-1&&j==dungeon[0].length-1){
            if(dungeon[i][j]>0){
                memo[i][j]=1;
            }else{
                memo[i][j]=-dungeon[i][j]+1;
            }
            return memo[i][j];
        }
        int rightMin=dfs(dungeon,i+1,j,memo);
        int downMin=dfs(dungeon,i,j+1,memo);
        int needMin=Math.min(rightMin,downMin)-dungeon[i][j];
        if(needMin>0){
            memo[i][j]=needMin;
        }else{
            memo[i][j]=1;
        }
        return memo[i][j];
    }
}
```
### 解法三
`DFS`:
```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if(dungeon==null||dungeon.length==0||dungeon[0].length==0){
            return 0;
        }
        return dfs(dungeon,0,0);
    }
    public int dfs(int[][] dungeon,int i,int j){
        if(i>=dungeon.length||j>=dungeon[0].length){
            return Integer.MAX_VALUE;
        }
        if(i==dungeon.length-1&&j==dungeon[0].length-1){
            return dungeon[i][j]>0?1:-dungeon[i][j]+1;
        }
        int rightMin=dfs(dungeon,i+1,j);
        int downMin=dfs(dungeon,i,j+1);
        int needMin=Math.min(rightMin,downMin)-dungeon[i][j];
        return needMin>0?needMin:1;
    }
}
```
自己的写法：
```java
class Solution {
    int ans=Integer.MIN_VALUE;
    public int calculateMinimumHP(int[][] dungeon) {
        dfs(dungeon,0,Integer.MAX_VALUE,0,0);
        if(ans>=0){
            return 1;
        }else{
            return -1*ans+1;
        }
    }
    public void dfs(int[][] nums,int sum,int minSum,int i,int j){
        if(i==nums.length-1&&j==nums[0].length-1){
            sum+=nums[i][j];
            minSum=sum<minSum?sum:minSum;
            ans=ans<minSum?minSum:ans;
            return;
        }
        if(i<nums.length-1){
            int tempSum=sum+nums[i][j];
            int tempMinSum=tempSum<minSum?tempSum:minSum;
            dfs(nums,tempSum,tempMinSum,i+1,j);
        }
        if(j<nums[0].length-1){
            int tempSum=sum+nums[i][j];
            int tempMinSum=tempSum<minSum?tempSum:minSum;
            dfs(nums,tempSum,tempMinSum,i,j+1);
        }
    }
}
```
