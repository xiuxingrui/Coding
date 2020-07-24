# [LeetCode74.搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)
## 题目描述
编写一个高效的算法来判断 `m x n` 矩阵中，是否存在一个目标值。该矩阵具有如下特性：
- 每行中的整数从左到右按升序排列。
- 每行的第一个整数大于前一行的最后一个整数。

### 示例
```
输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
输出: true
```
```
输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
输出: false
```
## 题解
### 解法一
左下角写法：
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m=matrix.length;
        if(m==0){
            return false;
        }
        int n=matrix[0].length;
        if(n==0){
            return false;
        }
        int i=m-1,j=0;
        while(i>=0&&j<n){
            if(matrix[i][j]==target){
                return true;
            }else if(matrix[i][j]>target){
                i--;
            }else{
                j++;
            }
        }
        return false;
    }
}
```
右上角写法:
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m=matrix.length;
        if(m==0){
            return false;
        }
        int n=matrix[0].length;
        if(n==0){
            return false;
        }
        int i=0,j=n-1;
        while(i<m&&j>=0){
            if(matrix[i][j]==target){
                return true;
            }else if(matrix[i][j]>target){
                j--;
            }else{
                i++;
            }
        }
        return false;
    }
}
```
### 解法二
自己解法
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m=matrix.length;
        if(m==0){
            return false;
        }
        int n=matrix[0].length;
        if(n==0){
            return false;
        }
        if(target<matrix[0][0]||target>matrix[m-1][n-1]){
            return false;
        }   
        int left=0,right=m-1;
        while(left<right){
            int mid=left+(right-left)/2;
            if(matrix[mid][n-1]>=target){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        int min=left;
        left=0;
        right=n-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(matrix[min][mid]==target){
                return true;
            }else if(matrix[min][mid]<target){
                left=mid+1;
            }else{
                right=mid-1;
            }
        }
        return false;
    }
}
```
