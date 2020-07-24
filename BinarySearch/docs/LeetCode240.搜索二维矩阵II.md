# [LeetCode240.搜索二维矩阵II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)
## 题目描述
编写一个高效的算法来搜索 `m x n` 矩阵 `matrix` 中的一个目标值 `target`。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

### 示例
现有矩阵 `matrix` 如下：
```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
- 给定 `target = 5`，返回 `true`。
- 给定 `target = 20`，返回 `false`。

## 题解
### 解法一：
标志数引入： 此类矩阵中左下角和右上角元素有特殊性，称为标志数。

- 左下角元素： 为所在列最大元素，所在行最小元素。
- 右上角元素： 为所在行最大元素，所在列最小元素。

标志数性质： 将 `matrix` 中的左下角元素（标志数）记作 `flag` ，则有:
- 若 `flag > target` ，则 `target` 一定在 `flag` 所在行的上方，即 `flag` 所在行可被消去。
- 若 `flag < target` ，则 `target` 一定在 `flag` 所在列的右方，即 `flag`所在列可被消去。

本题解以左下角元素为例，同理，右上角元素 也具有行（列）消去的性质。

算法流程： 根据以上性质，设计算法在每轮对比时消去一行（列）元素，以降低时间复杂度。

1. 从矩阵 `matrix` 左下角元素（索引设为 `(i, j)`）开始遍历，并与目标值对比：
   - 当 `matrix[i][j] > target` 时： 行索引向上移动一格（即 `i--`），即消去矩阵第 `i` 行元素；
   - 当 `matrix[i][j] < target` 时： 列索引向右移动一格（即 `j++`），即消去矩阵第 `j` 列元素；
   - 当 `matrix[i][j] == target` 时： 返回 `true` 。

2. 若行索引或列索引越界，则代表矩阵中无目标值，返回 `false` 。
左下角写法：
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int i = matrix.length - 1, j = 0;
        while(i >= 0 && j < matrix[0].length)
        {
            if(matrix[i][j] > target) i--;
            else if(matrix[i][j] < target) j++;
            else return true;
        }
        return false;
    }
}
```
右上角写法：
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int rows = matrix.length, columns = matrix[0].length;
        int row = 0, column = columns - 1;
        while (row < rows && column >= 0) {
            int num = matrix[row][column];
            if (num == target) {
                return true;
            } else if (num > target) {
                column--;
            } else {
                row++;
            }
        }
        return false;
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(M+N)$：其中，`N` 和 `M` 分别为矩阵行数和列数，此算法最多循环 `M+N` 次。
- 空间复杂度 $O(1)$: `i`, `j` 指针使用常数大小额外空间。


### 解法二：
自己解法:
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix.length==0){
            return false;
        }
        if(matrix[0].length==0){
            return false;
        }
        int m=matrix.length,n=matrix[0].length;
        int left=0,right=m;
        while(left<right){
            int mid=left+(right-left)/2;
            if(matrix[mid][0]>target){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        if(left==0){
            return false;
        }
        int maxLeft=left-1;
        left=0;
        right=m;
        while(left<right){
            int mid=left+(right-left)/2;
            if(matrix[mid][n-1]>=target){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        if(left==m){
            return false;
        }
        int minLeft=left;
        for(int i=minLeft;i<=maxLeft;i++){
            left=0;
            right=n-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(matrix[i][mid]==target){
                    return true;
                }else if(matrix[i][mid]>target){
                    right=mid-1;
                }else{
                    left=mid+1;
                }
            }
        }
        return false;
    }
}
```
