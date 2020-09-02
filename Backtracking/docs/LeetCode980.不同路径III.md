# [LeetCode980.不同路径III](https://leetcode-cn.com/problems/unique-paths-iii/)
## 题目描述
在二维网格 `grid` 上，有 4 种类型的方格：

- 1 表示起始方格。且只有一个起始方格。
- 2 表示结束方格，且只有一个结束方格。
- 0 表示我们可以走过的空方格。
- -1 表示我们无法跨越的障碍。

返回在四个方向（上、下、左、右）上行走时，从起始方格到结束方格的不同路径的数目。

每一个无障碍方格都要通过一次，但是一条路径中不能重复通过同一个方格。

- `1 <= grid.length * grid[0].length <= 20`
### 示例
```
输入：[[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
输出：2
解释：我们有以下两条路径：
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
```
```
输入：[[1,0,0,0],[0,0,0,0],[0,0,0,2]]
输出：4
解释：我们有以下四条路径： 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
```
```
输入：[[0,1],[2,0]]
输出：0
解释：
没有一条路能完全穿过每一个空的方格一次。
请注意，起始和结束方格可以位于网格中的任意位置。
```
## 题解
```java
class Solution {
    int ans=0;
    public int uniquePathsIII(int[][] grid) {
        int m=grid.length;
        int n=grid[0].length;
        boolean[][] visited=new boolean[m][n];
        int sx=0,sy=0,ex=0,ey=0;
        int count=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]!=-1){
                    count++;
                }
                if(grid[i][j]==1){
                    sx=i;
                    sy=j;
                }
                if(grid[i][j]==2){
                    ex=i;
                    ey=j;
                }
            }
        }
        visited[sx][sy]=true;
        dfs(grid,visited,1,count,sx,sy,ex,ey);
        return ans;
    }
    public void dfs(int[][] nums,boolean[][] visited,int cnt,int count,int i,int j,int ex,int ey){
        if(i==ex&&j==ey&&cnt==count){
            ans++;
            return;
        }
        if(j<nums[0].length-1&&nums[i][j+1]!=-1&&visited[i][j+1]==false){
            visited[i][j+1]=true;
            dfs(nums,visited,cnt+1,count,i,j+1,ex,ey);
            visited[i][j+1]=false;
        }
        if(j>0&&nums[i][j-1]!=-1&&visited[i][j-1]==false){
            visited[i][j-1]=true;
            dfs(nums,visited,cnt+1,count,i,j-1,ex,ey);
            visited[i][j-1]=false;
        }
        if(i<nums.length-1&&nums[i+1][j]!=-1&&visited[i+1][j]==false){
            visited[i+1][j]=true;
            dfs(nums,visited,cnt+1,count,i+1,j,ex,ey);
            visited[i+1][j]=false;
        }
        if(i>0&&nums[i-1][j]!=-1&&visited[i-1][j]==false){
            visited[i-1][j]=true;
            dfs(nums,visited,cnt+1,count,i-1,j,ex,ey);
            visited[i-1][j]=false;
        }
    }
}
```