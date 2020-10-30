# [LeetCode463.岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)
## 题目描述
给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

### 示例
```
输入:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

输出: 16

解释: 它的周长是下面图片中的 16 个黄色的边：
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201030004637.png)

## 题解
### 解法一
对于一个陆地格子的每条边，它被算作岛屿的周长当且仅当这条边为网格的边界或者相邻的另一个格子为水域。 因此，我们可以遍历每个陆地格子，看其四个方向是否为边界或者水域，如果是，将这条边的贡献（即 1）加入答案 `ans` 中即可。
```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        int[][] dirs={{0,1},{0,-1},{1,0},{-1,0}};
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (grid[i][j] == 1) {
                    int cnt = 0;
                    for (int k = 0; k < 4; ++k) {
                        int tx = i + dirs[k][0];
                        int ty = j + dirs[k][1];
                        if (tx < 0 || tx >= n || ty < 0 || ty >= m || grid[tx][ty] == 0) {
                            cnt += 1;
                        }
                    }
                    ans += cnt;
                }
            }
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nm)$，其中 `n` 为网格的高度，`m` 为网格的宽度。我们需要遍历每个格子，每个格子要看其周围 4 个格子是否为岛屿，因此总时间复杂度为 $O(4nm)=O(nm)$。
- 空间复杂度：$O(1)$。只需要常数空间存放若干变量。

### 解法二
`DFS`

我们也可以将方法一改成深度优先搜索遍历的方式，此时遍历的方式可扩展至统计多个岛屿各自的周长。需要注意的是为了防止陆地格子在深度优先搜索中被重复遍历导致死循环，我们需要将遍历过的陆地格子标记为已经遍历过，下面的代码中我们设定值为 222 的格子为已经遍历过的陆地格子。

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (grid[i][j] == 1) {
                    ans += dfs(i, j, grid, n, m);
                }
            }
        }
        return ans;
    }

    public int dfs(int x, int y, int[][] grid, int n, int m) {
        if (x < 0 || x >= n || y < 0 || y >= m || grid[x][y] == 0) {
            return 1;
        }
        if (grid[x][y] == 2) {
            return 0;
        }
        int[][] dirs={{0,1},{0,-1},{1,0},{-1,0}};
        grid[x][y] = 2;
        int res = 0;
        for (int i = 0; i < 4; ++i) {
            int tx = x + dirs[i][0];
            int ty = y + dirs[i][1];
            res += dfs(tx, ty, grid, n, m);
        }
        return res;
    }
}
```
或
```java
class Solution {
    int len=0;
    public int islandPerimeter(int[][] grid) {
        if(grid==null||grid.length==0||grid[0].length==0){
            return 0;
        }
        int m=grid.length,n=grid[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1){
                    dfs(grid,i,j);
                    return len;
                }
            }
        }
        return 0;
    }
    public void dfs(int[][] grid,int i,int j){
        if(i<0||i>=grid.length||j<0||j>=grid[0].length||grid[i][j]!=1){
            return;
        }
        int[][] dirs={{0,1},{0,-1},{1,0},{-1,0}};
        grid[i][j]=2;
        for(int k=0;k<4;k++){
            int newI=i+dirs[k][0];
            int newJ=j+dirs[k][1];
            if(newI<0||newI>=grid.length||newJ<0||newJ>=grid[0].length||grid[newI][newJ]!=1&&grid[newI][newJ]!=2){
                len++;
            }
        }
        for(int k=0;k<4;k++){
            int newI=i+dirs[k][0];
            int newJ=j+dirs[k][1];
            dfs(grid,newI,newJ);
        }
        
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nm)$，其中 `n` 为网格的高度，`m` 为网格的宽度。每个格子至多会被遍历一次，因此总时间复杂度为 $O(nm)$。
- 空间复杂度：$O(nm)$。深度优先搜索复杂度取决于递归的栈空间，而栈空间最坏情况下会达到 $O(nm)$。
### 解法三
`BFS`

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        if(grid==null||grid.length==0||grid[0].length==0){
            return 0;
        }
        int m=grid.length,n=grid[0].length;
        int[][] dirs={{0,1},{0,-1},{1,0},{-1,0}};
        int ans=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1){
                    Queue<int[]> queue=new LinkedList<>();
                    grid[i][j]=2;
                    queue.offer(new int[]{i,j});
                    while(!queue.isEmpty()){
                        int[] temp=queue.poll();
                        int curI=temp[0],curJ=temp[1];
                        for(int k=0;k<4;k++){
                            int newI=curI+dirs[k][0];
                            int newJ=curJ+dirs[k][1];
                            if(newI<0||newI>=grid.length||newJ<0||newJ>=grid[0].length||grid[newI][newJ]!=1&&grid[newI][newJ]!=2){
                                ans++;
                            }
                        }
                        for(int k=0;k<4;k++){
                            int newI=curI+dirs[k][0];
                            int newJ=curJ+dirs[k][1];
                            if(newI>=0&&newI<grid.length&&newJ>=0&&newJ<grid[0].length&&grid[newI][newJ]==1){
                                grid[newI][newJ]=2;//入队前就要修改否则会多计算
                                queue.offer(new int[]{newI,newJ});
                            }
                        }
                    }
                    return ans;
                }
            }
        }
        return 0;
    }
}
```