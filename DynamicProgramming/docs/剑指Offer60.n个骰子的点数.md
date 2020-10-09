# [剑指Offer60.n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)
## 题目描述
把`n`个骰子扔在地上，所有骰子朝上一面的点数之和为`s`。输入`n`，打印出`s`的所有可能的值出现的概率。

你需要用一个浮点数数组返回答案，其中第 `i` 个元素代表这 `n` 个骰子所能掷出的点数集合中第 `i` 小的那个的概率。

- `1 <= n <= 11`
### 示例
```
输入: 1
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
```
```
输入: 2
输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
```
## 题解
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009140843.png)


```java
class Solution {
    public double[] twoSum(int n) {
        int[][] dp=new int[n+1][6*n+1];
        for(int i=1;i<=6;i++){
            dp[1][i]=1;
        }
        for(int i=2;i<=n;i++){
            for(int j=i;j<=6*i;j++){
                for(int k=1;k<=6;k++){
                    if(j-k>=0){
                        dp[i][j]+=dp[i-1][j-k];
                    }
                }
                // for(int k=Math.max(1,j-6*i+6);k<=Math.min(6,j-i+1);k++){//确定上下界
                //     dp[i][j]+=dp[i-1][j-k];
                // }
                // for(int k=1;k<=Math.min(6,j-i+1);k++){//确定上界
                //     dp[i][j]+=dp[i-1][j-k];
                // }
            }
        }
        double total = (double)Math.pow(6,n);
        double[] ans = new double[5*n+1];
        for(int i=n;i<=6*n;i++){
            ans[i-n]=dp[n][i]/total;
        }
        return ans;
    }
}
```
优化存储空间：
```java
class Solution {
    public double[] twoSum(int n) {
        int[] dp=new int[6*n+1];
        for(int i=1;i<=6;i++){
            dp[i]=1;
        }
        for(int i=2;i<=n;i++){
            for(int j=6*i;j>=i;j--){
                dp[j]=0;
                for(int k=1;k<=6;k++){
                    if(j-k>=i-1){
                        dp[j]+=dp[j-k];
                    }
                }
            }
        }
        double total = (double)Math.pow(6,n);
        double[] ans = new double[5*n+1];
        for(int i=n;i<=6*n;i++){
            ans[i-n]=dp[i]/total;
        }
        return ans;
    }
}
```
