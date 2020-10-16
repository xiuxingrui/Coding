# [LeetCode200.岛屿数量]()
## 题目描述
给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` 的值为 `'0'` 或 `'1'`
### 示例
```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```
```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```
## 题解
### 解法一
### 解法二
`DFS`:
目标是找到矩阵中 “岛屿的数量” ，上下左右相连的 1 都被认为是连续岛屿。

`dfs`方法： 设目前指针指向一个岛屿中的某一点 `(i, j)`，寻找包括此点的岛屿边界。
- 从 `(i, j)` 向此点的上下左右 `(i+1,j)`,`(i-1,j)`,`(i,j+1)`,`(i,j-1)` 做深度搜索。
- 终止条件：
  - `(i, j)` 越过矩阵边界;
  - `grid[i][j] == 0`，代表此分支已越过岛屿边界。
- 搜索岛屿的同时，执行 `grid[i][j] = '0'`，即将岛屿所有节点删除，以免之后重复搜索相同岛屿。
主循环：

遍历整个矩阵，当遇到 `grid[i][j] == '1'` 时，从此点开始做深度优先搜索 `dfs`，岛屿数 `count + 1` 且在深度优先搜索中删除此岛屿。

最终返回岛屿数 `count` 即可。
```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[0].length; j++) {
                if(grid[i][j] == '1'){
                    dfs(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }
    private void dfs(char[][] grid, int i, int j){
        if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] == '0') return;
        grid[i][j] = '0';
        dfs(grid, i + 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i - 1, j);
        dfs(grid, i, j - 1);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(MN)$，其中 `M` 和 `N` 分别为行数和列数。
- 空间复杂度：$O(MN)$，在最坏情况下，整个网格均为陆地，深度优先搜索的深度达到 `MN`。
### 解法三
`BFS`:
同样地，我们也可以使用广度优先搜索代替深度优先搜索。

为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 1，则将其加入队列，开始进行广度优先搜索。在广度优先搜索的过程中，每个搜索到的 1 都会被重新标记为 0。直到队列为空，搜索结束。

最终岛屿的数量就是我们进行广度优先搜索的次数。
```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[0].length; j++) {
                if(grid[i][j] == '1'){
                    bfs(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }
    private void bfs(char[][] grid, int i, int j){
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] { i, j });
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            i = cur[0]; j = cur[1];
            if(0 <= i && i < grid.length && 0 <= j && j < grid[0].length && grid[i][j] == '1') {
                grid[i][j] = '0';//如果在出队时才标记为'0'，则会陷入死循环
                queue.offer(new int[] { i + 1, j });
                queue.offer(new int[] { i - 1, j });
                queue.offer(new int[] { i, j + 1 });
                queue.offer(new int[] { i, j - 1 });
            }
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(MN)$，其中 `M` 和 `N` 分别为行数和列数。
- 空间复杂度：$O(min(M,N))$，在最坏情况下，整个网格均为陆地，队列的大小可以达到 $min(M,N)$。