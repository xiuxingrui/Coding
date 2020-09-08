# [LeetCode1035.不相交的线](https://leetcode-cn.com/problems/uncrossed-lines/)
## 题目描述
我们在两条独立的水平线上按给定的顺序写下 `A` 和 `B` 中的整数。

现在，我们可以绘制一些连接两个数字 `A[i]` 和 `B[j]` 的直线，只要 `A[i] == B[j]`，且我们绘制的直线不与任何其他连线（非水平线）相交。

以这种方法绘制线条，并返回我们可以绘制的最大连线数。

 
### 示例

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200908213649.png)

```
输入：A = [1,4,2], B = [1,2,4]
输出：2
解释：
我们可以画出两条不交叉的线，如上图所示。
我们无法画出第三条不相交的直线，因为从 A[1]=4 到 B[2]=4 的直线将与从 A[2]=2 到 B[1]=2 的直线相交。
```
```
输入：A = [2,5,1,2,5], B = [10,5,2,1,5,2]
输出：3
```
```
输入：A = [1,3,7,1,7,5], B = [1,9,2,5,1]
输出：2
```
## 题解
从图中可以看出，如果想要不相交，则必然相对位置要一致，换句话说就是：公共子序列。因此和`1143.最长公共子序列` 一样，属于换皮题，代码也是一模一样。

```java
class Solution {
    public int maxUncrossedLines(int[] A, int[] B) {
        int lenA=A.length,lenB=B.length;
        int[][] dp=new int[lenA+1][lenB+1];
        for(int i=1;i<lenA+1;i++){
            for(int j=1;j<lenB+1;j++){
                if(A[i-1]==B[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=Math.max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[lenA][lenB];
    }
}
```


