# [剑指Offer29.顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)
## 题目描述
给定一个包含 `m x n` 个元素的矩阵（`m` 行, `n` 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

### 示例
```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```
## 题解
```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        int m=matrix.length;
        if(m==0){
            return new int[0];
        }
        int n=matrix[0].length;
        if(n==0){
            return new int[0];
        }
        int[] res=new int[m*n];
        int l=0,r=n-1,t=0,b=m-1,cnt=0;
        while(true){
            for(int i=l;i<=r;i++){
                res[cnt++]=matrix[t][i];
            }
            if(++t>b){
                break;
            }
            for(int i=t;i<=b;i++){
                res[cnt++]=matrix[i][r];
            }
            if(--r<l){
                break;
            }
            for(int i=r;i>=l;i--){
                res[cnt++]=matrix[b][i];
            }
            if(--b<t){
                break;
            }
            for(int i=b;i>=t;i--){
                res[cnt++]=matrix[i][l];
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
- 时间复杂度 $O(MN)$： `M`,`N`分别为矩阵行数和列数。
- 空间复杂度 $O(1)$： 四个边界 `l` , `r` , `t` , `b` 使用常数大小的 额外 空间（ `res` 为必须使用的空间）。
