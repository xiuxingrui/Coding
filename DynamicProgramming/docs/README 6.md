# [LeetCode221.最大正方形](https://leetcode-cn.com/problems/maximal-square/)
## 题目描述
在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。
### 示例
```
输入: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```
## 题解
在其他动态规划方法的题解中，大都会涉及到下列形式的代码：
```java
// 伪代码
if (matrix(i - 1, j - 1) == '1') {
    dp(i, j) = min(dp(i - 1, j), dp(i, j - 1), dp(i - 1, j - 1)) + 1;
}
```
其中，`dp(i, j)` 是以 `matrix(i - 1, j - 1)` 为 右下角 的正方形的最大边长。等同于：`dp(i + 1, j + 1)` 是以 `matrix(i, j)` 为右下角的正方形的最大边长

若某格子值为 1，则以此为右下角的正方形的、最大边长为：上面的正方形、左面的正方形或左上的正方形中，最小的那个，再加上此格。

先来阐述简单共识

- 若形成正方形（非单 1），以当前为右下角的视角看，则需要：当前格、上、左、左上都是 1
- 可以换个角度：当前格、上、左、左上都不能受 0 的限制，才能成为正方形

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200910210200.png)

上面详解了 三者取最小 的含义：

- 图 1：受限于左上的 0
- 图 2：受限于上边的 0
- 图 3：受限于左边的 0
- 数字表示：以此为正方形右下角的最大边长
- 黄色表示：格子 ? 作为右下角的正方形区域

就像 木桶的短板理论 那样——附近的最小边长，才与 ? 的最长边长有关。此时已可得到递推公式
```java
// 伪代码
if (grid[i - 1][j - 1] == '1') {
    dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]) + 1;
}
```
```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int maxSide = 0;
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return maxSide;
        }
        int rows = matrix.length, columns = matrix[0].length;
        int[][] dp = new int[rows][columns];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                    }
                    maxSide = Math.max(maxSide, dp[i][j]);
                }
            }
        }
        int maxSquare = maxSide * maxSide;
        return maxSquare;
    }
}
```
自己的写法
```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return 0;
        }
        int m=matrix.length;
        int n=matrix[0].length;
        int[][] dp=new int[m][n];
        int ans=0;
        for(int i=0;i<n;i++){
            if(matrix[0][i]=='1'){
                dp[0][i]=1;
                ans=1;
            }
        }
        for(int i=0;i<m;i++){
            if(matrix[i][0]=='1'){
                dp[i][0]=1;
                ans=1;
            }
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(matrix[i][j]=='1'){
                    if(dp[i-1][j-1]>0&&dp[i][j-1]>0&&dp[i-1][j]>0){
                        int len=Math.min(dp[i-1][j-1],Math.min(dp[i][j-1],dp[i-1][j]));
                        dp[i][j]=len+1;
                    }else{
                        dp[i][j]=1;
                    }
                    ans=ans>dp[i][j]?ans:dp[i][j];
                }
            }
        }
        return ans*ans;
    }  
}
```
### 复杂度分析
- 时间复杂度 $O(height∗width)$
- 空间复杂度 $O(height∗width)$
