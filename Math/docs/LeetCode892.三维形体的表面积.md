# [LeetCode892.三维形体的表面积](https://leetcode-cn.com/problems/surface-area-of-3d-shapes/)
## 题解
在 `N*N` 的网格上，我们放置一些 1 * 1 * 1  的立方体。

每个值 `v = grid[i][j]` 表示 `v` 个正方体叠放在对应单元格 `(i, j)` 上。

请你返回最终形体的表面积。

- `1 <= N <= 50`
- `0 <= grid[i][j] <= 50`

### 示例
```
输入：[[2]]
输出：10
```
```
输入：[[1,2],[3,4]]
输出：34
```
```
输入：[[1,0],[0,2]]
输出：16
```
```
输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：32
```
```
输入：[[2,2,2],[2,1,2],[2,2,2]]
输出：46
```
## 题解
```java
class Solution {
    public int surfaceArea(int[][] grid) {
        int cnt=0,n=grid.length;
        int[][] dirs={{0,1},{0,-1},{1,0},{-1,0}};
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]!=0){
                    cnt+=2+grid[i][j]*4;
                    for(int k=0;k<4;k++){
                        int newI=i+dirs[k][0],newJ=j+dirs[k][1];
                        if(newI>=0&&newI<n&&newJ>=0&&newJ<n&&grid[newI][newJ]!=0){
                            cnt-=Math.min(grid[i][j],grid[newI][newJ]);
                        }
                    }
                }
            }
        }
        return cnt;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N^2)$，其中 `N` 是 `grid` 中的行和列的数目。
- 空间复杂度：$O(1)$。