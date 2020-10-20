# [LeetCode695.岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)
## 题目描述
给定一个包含了一些 0 和 1 的非空二维数组 `grid` 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 `grid` 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

- 注意: 给定的矩阵`grid` 的长度和宽度都不超过 50。
### 示例
```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
对于上面这个给定矩阵应返回 6。注意答案不应该是 11 ，因为岛屿只能包含水平或垂直的四个方向的 1 。
```
```
[[0,0,0,0,0,0,0,0]]
对于上面这个给定的矩阵, 返回 0。
```
## 题解
### 解法一
- 我们想知道网格中每个连通形状的面积，然后取最大值。
- 如果我们在一个土地上，以 4 个方向探索与之相连的每一个土地（以及与这些土地相连的土地），那么探索过的土地总数将是该连通形状的面积。
- 为了确保每个土地访问不超过一次，我们每次经过一块土地时，将这块土地的值置为 0。这样我们就不会多次访问同一土地。

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int ans=0;
        int m=grid.length,n=grid[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1){
                    ans=Math.max(ans,getArea(grid,i,j));
                }
            }
        }
        return ans;
    }
    public int getArea(int grid[][],int i,int j){
        if(i<0||i>=grid.length||j<0||j>=grid[0].length||grid[i][j]==0){
            return 0;
        }
        grid[i][j]=0;
        return 1+getArea(grid,i-1,j)+getArea(grid,i+1,j)+getArea(grid,i,j-1)+getArea(grid,i,j+1);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(R*C)$。其中 `R` 是给定网格中的行数，`C` 是列数。我们访问每个网格最多一次。
- 空间复杂度：$O(R*C)$，递归的深度最大可能是整个网格的大小，因此最大可能使用 $O(R*C)$的栈空间。
### 解法二
我们把方法二中的栈改为队列，每次从队首取出土地，并将接下来想要遍历的土地放在队尾，就实现了广度优先搜索算法。
```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int ans=0;
        int m=grid.length,n=grid[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1){
                    ans=Math.max(ans,getArea(grid,i,j));
                }
            }
        }
        return ans;
    }
    public int getArea(int grid[][],int i,int j){
        Queue<int[]> queue=new LinkedList<>();
        queue.offer(new int[]{i,j});
        int area=0;
        while(!queue.isEmpty()){
            int[] temp=queue.poll();
            if(temp[0]>=0&&temp[0]<grid.length&&temp[1]>=0&&temp[1]<grid[0].length&&grid[temp[0]][temp[1]]==1){
                area++;
                grid[temp[0]][temp[1]]=0;
                queue.offer(new int[]{temp[0]-1,temp[1]});
                queue.offer(new int[]{temp[0]+1,temp[1]});
                queue.offer(new int[]{temp[0],temp[1]-1});
                queue.offer(new int[]{temp[0],temp[1]+1});
            }
        }
        return area;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(R*C)$。其中 `R` 是给定网格中的行数，`C` 是列数。我们访问每个网格最多一次。
- 空间复杂度：$O(R*C)$，队列中最多会存放所有的土地，土地的数量最多为`R*C` 块，因此使用的空间为$O(R*C)$。
