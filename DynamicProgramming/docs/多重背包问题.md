# 多重背包问题
## 题目描述
有 `N` 种物品和一个容量是 `V` 的背包。

第 `i` 种物品最多有 $s_{i}$ 件，每件体积是 $v_{i}$，价值是 $w_{i}$。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。输出最大价值。
## 题解
### 解法一:朴素解法
```java
public class Solution {
    
    public int backPackII(int m, int[] A, int[] V,int[] S) {
        if(A==null||A.length==0||V==null||V.length==0||m==0){
            return 0;
        }
        int[] dp=new int[m+1];
        for(int i=0;i<A.length;i++){
            for(int j=m;j>=A[i];j--){
                for(int k=0;k<=S[i]&&k*A[i]<=j;k++)
                dp[j]=Math.max(dp[j],dp[j-k*A[i]]+k*V[i]);
                
            }
        }
        return dp[m];
    }
}
```
