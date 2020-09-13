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
- 从上述图解中，我们似乎得到的只是「动态规划 推进 的过程」，即如何从前面的 `dp` 推出后面的 `dp`，甚至还只是感性理解
- 距离代码我们还缺：`dp` 具体定义如何，数组多大，初值如何，如何与题目要求的面积相关
- `dp` 具体定义：`dp[i + 1][j + 1]` 表示 「以第 `i` 行、第 `j` 列为右下角的正方形的最大边长」

- 回到图解中，任何一个正方形，我们都「依赖」当前格 左、上、左上三个方格的情况
- 但第一行的上层已经没有格子，第一列左边已经没有格子，需要做特殊 `if` 判断来处理
- 为了代码简洁，我们 假设补充 了多一行全 '0'、多一列全 '0'

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200913195225.png)

- 此时 `dp` 数组的大小也明确为 `new dp[height + 1][width + 1]`
- 初始值就是将第一列 `dp[row][0]` 、第一行 `dp[0][col]` 都赋为 0，相当于已经计算了所有的第一行、第一列的 `dp` 值
- 题目要求面积。根据 「面积 = 边长 x 边长」可知，我们只需求出 最大边长 即可
- 定义 `maxSide` 表示最长边长，每次得出一个 `dp`，就 `maxSide = max(maxSide, dp);`
- 最终返回 `return maxSide * maxSide;`

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        // base condition
        if (matrix == null || matrix.length < 1 || matrix[0].length < 1) return 0;

        int height = matrix.length;
        int width = matrix[0].length;
        int maxSide = 0;

        // 相当于已经预处理新增第一行、第一列均为0
        int[][] dp = new int[height + 1][width + 1];

        for (int row = 1; row <=height; row++) {
            for (int col = 1; col <=width; col++) {
                if (matrix[row-1][col-1] == '1') {
                    dp[row][col] = Math.min(Math.min(dp[row-1][col], dp[row][col-1]), dp[row-1][col-1]) + 1;
                    maxSide = Math.max(maxSide, dp[row][col]);
                }
            }
        }
        return maxSide * maxSide;
    }
}
```
优化存储空间
```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        // base condition
        if (matrix == null || matrix.length < 1 || matrix[0].length < 1) return 0;

        int height = matrix.length;
        int width = matrix[0].length;
        int maxSide = 0;

        // 相当于已经预处理新增第一行、第一列均为0
        int[] dp = new int[width + 1];
        for (int row = 1; row <=height; row++) {
            int lastValue=dp[0];
            for (int col = 1; col <=width; col++) {
                int temp=dp[col];
                if (matrix[row-1][col-1] == '1') {
                    dp[col] = Math.min(Math.min(dp[col], dp[col-1]),lastValue) + 1;
                    maxSide = Math.max(maxSide,dp[col]);
                }else{//优化后必须有的一步
                    dp[col]=0;
                }
                lastValue=temp;
            }
        }
        return maxSide * maxSide;
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
空间优化
```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return 0;
        }
        int m=matrix.length;
        int n=matrix[0].length;
        int[] dp=new int[n];
        int ans=0;
        for(int i=0;i<n;i++){
            if(matrix[0][i]=='1'){
                dp[i]=1;
                ans=1;
            }
        }
        for(int i=1;i<m;i++){
            int lastValue=dp[0];
            dp[0]=matrix[i][0]=='1'?1:0;
            if(dp[0]==1){
                ans=ans>1?ans:1;
            }
            for(int j=1;j<n;j++){
                int temp=dp[j];
                if(matrix[i][j]=='1'){
                    if(matrix[i-1][j-1]=='1'&&matrix[i][j-1]=='1'&&matrix[i-1][j]=='1'){
                        int len=Math.min(lastValue,Math.min(dp[j-1],dp[j]));
                        dp[j]=len+1;
                    }else{
                        dp[j]=1;
                    }
                    ans=ans>dp[j]?ans:dp[j];
                }
                lastValue=temp;
            }
        }
        return ans*ans;
    }  
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

### 复杂度分析
- 时间复杂度 $O(height∗width)$
- 空间复杂度 $O(height∗width)$
