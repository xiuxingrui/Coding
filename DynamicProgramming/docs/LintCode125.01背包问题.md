# [LintCode125.01背包问题](https://www.lintcode.com/problem/backpack-ii/description)
## 题目描述
有 `n` 个物品和一个大小为 `m` 的背包. 给定数组 `A`表示每个物品的大小和数组 `V` 表示每个物品的价值。

每个物品仅可使用一次。
## 题解
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @param V: Given n items with value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int[] V) {
        if(A==null||A.length==0||V==null||V.length==0||m==0){
            return 0;
        }
        int[][] dp=new int[A.length+1][m+1];
        for(int i=1;i<=A.length;i++){
            for(int j=0;j<=m;j++){
                dp[i][j]=dp[i-1][j];
                if(A[i-1]<=j){
                    dp[i][j]=Math.max(dp[i-1][j],dp[i-1][j-A[i-1]]+V[i-1]);
                }
            }
        }
        return dp[A.length][m];
    }
}
```
优化：
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @param V: Given n items with value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int[] V) {
        if(A==null||A.length==0||V==null||V.length==0||m==0){
            return 0;
        }
        int[] dp=new int[m+1];
        for(int i=1;i<=A.length;i++){
            for(int j=m;j>=A[i-1];j--){
                dp[j]=Math.max(dp[j],dp[j-A[i-1]]+V[i-1]);
                
            }
        }
        return dp[m];
    }
}
```