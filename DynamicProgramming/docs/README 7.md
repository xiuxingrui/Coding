# [LeetCode1277.统计全为1的正方形子矩阵](https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/)
## 题目描述
给你一个 `m * n` 的矩阵，矩阵中的元素不是 0 就是 1，请你统计并返回其中完全由 1 组成的 正方形 子矩阵的个数。

- `1 <= arr.length <= 300`
- `1 <= arr[0].length <= 300`
- `0 <= arr[i][j] <= 1`

### 示例
```
输入：matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
输出：15
解释： 
边长为 1 的正方形有 10 个。
边长为 2 的正方形有 4 个。
边长为 3 的正方形有 1 个。
正方形的总数 = 10 + 4 + 1 = 15.
```
```
输入：matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
输出：7
解释：
边长为 1 的正方形有 6 个。 
边长为 2 的正方形有 1 个。
正方形的总数 = 6 + 1 = 7.
```
## 题解
```java
class Solution {
    public int countSquares(int[][] matrix) {
        int rows = matrix.length, columns = matrix[0].length;
        int[][] dp = new int[rows][columns];
        int ans=0;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == 1) {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                    }
                    ans+=dp[i][j];
                }
            }
        }
        return ans;
    }   
}
```
空间优化:
```java

自己的写法：
```java
class Solution {
    public int countSquares(int[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return 0;
        }
        int m=matrix.length;
        int n=matrix[0].length;
        int[][] dp=new int[m][n];
        int ans=0;
        for(int i=0;i<n;i++){
            if(matrix[0][i]==1){
                dp[0][i]=1;
                ans++;
            }
        }
        for(int i=1;i<m;i++){
            if(matrix[i][0]==1){
                dp[i][0]=1;
                ans++;
            }
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(matrix[i][j]==1){
                    ans++;
                    if(dp[i-1][j-1]>0&&dp[i][j-1]>0&&dp[i-1][j]>0){
                        int len=Math.min(dp[i-1][j-1],Math.min(dp[i][j-1],dp[i-1][j]));
                        dp[i][j]=len+1;
                        ans+=dp[i][j]-1;
                    }else{
                        dp[i][j]=1;
                    }
                }
            }
        }
        return ans;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(MN)$。
- 空间复杂度：$O(MN)$。由于递推式中 `f[i][j]` 只与本行和上一行的若干个值有关，因此空间复杂度可以优化至 $O(N)$。
