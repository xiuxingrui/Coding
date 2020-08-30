# [LintCode92.背包问题](https://www.lintcode.com/problem/backpack/description)
## 题目描述
在`n`个物品中挑选若干物品装入背包，最多能装多满？假设背包的大小为`m`，每个物品的大小为`A[i]`
## 题解
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        if(A==null||A.length==0||m==0){
            return 0;
        }
        int[][] dp=new int[A.length+1][m+1];
        for(int i=1;i<=A.length;i++){
            for(int j=0;j<=m;j++){
                dp[i][j]=dp[i-1][j];
                if(j>=A[i-1]){
                    dp[i][j]=Math.max(dp[i-1][j],dp[i-1][j-A[i-1]]+A[i-1]);
                }
            }
        }
        return dp[A.length][m];
    }
}
```
优化:
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        if(A==null||A.length==0||m==0){
            return 0;
        }
        int[] dp=new int[m+1];
        for(int i=1;i<=A.length;i++){
            for(int j=m;j>=A[i-1];j--){
                    dp[j]=Math.max(dp[j],dp[j-A[i-1]]+A[i-1]);
            }
        }
        return dp[m];
    }
}
```
