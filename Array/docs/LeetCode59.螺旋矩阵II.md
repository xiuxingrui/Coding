# [LeetCode59.螺旋矩阵II](https://leetcode-cn.com/problems/spiral-matrix-ii/)
## 题目描述
给定一个正整数 `n`，生成一个包含 `1` 到 $n^{2}$ 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

### 示例
```
输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```
## 题解
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res=new int[n][n];
        int l=0,r=n-1,t=0,b=n-1,num=1;
        while(true){
            for(int i=l;i<=r;i++){
                res[t][i]=num++;
            }
            if(++t>b){
                break;
            }
            for(int i=t;i<=b;i++){
                res[i][r]=num++;
            }
            if(--r<l){
                break;
            }
            for(int i=r;i>=l;i--){
                res[b][i]=num++;
            }
            if(--b<t){
                break;
            }
            for(int i=b;i>=t;i--){
                res[i][l]=num++;
            }
            if(++l>r){
                break;
            }
        }
        return res;
    }
}
```
### 复杂度分析：
- 时间复杂度 $O(N^2)$：`N`分别为矩阵行数和列数。
- 空间复杂度 $O(1)$： 四个边界 `l` , `r` , `t` , `b` 使用常数大小的 额外 空间（ `res` 为必须使用的空间）。
