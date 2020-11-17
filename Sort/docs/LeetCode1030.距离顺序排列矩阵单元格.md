# [LeetCode1030.距离顺序排列矩阵单元格](https://leetcode-cn.com/problems/matrix-cells-in-distance-order/)
## 题目描述
给出 `R` 行 `C` 列的矩阵，其中的单元格的整数坐标为 `(r, c)`，满足 `0 <= r < R` 且 `0 <= c < C`。

另外，我们在该矩阵中给出了一个坐标为 `(r0, c0)` 的单元格。

返回矩阵中的所有单元格的坐标，并按到 `(r0, c0)` 的距离从最小到最大的顺序排，其中，两单元格`(r1, c1)` 和 `(r2, c2)` 之间的距离是曼哈顿距离，`|r1 - r2| + |c1 - c2|`。（你可以按任何满足此条件的顺序返回答案。）

- `1 <= R <= 100`
- `1 <= C <= 100`
- `0 <= r0 < R`
- `0 <= c0 < C`

### 示例
```
输入：R = 1, C = 2, r0 = 0, c0 = 0
输出：[[0,0],[0,1]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1]
```
```
输入：R = 2, C = 2, r0 = 0, c0 = 1
输出：[[0,1],[0,0],[1,1],[1,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2]
[[0,1],[1,1],[0,0],[1,0]] 也会被视作正确答案。
```
```
输入：R = 2, C = 3, r0 = 1, c0 = 2
输出：[[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2,2,3]
其他满足题目要求的答案也会被视为正确，例如 [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]]。
```
## 题解
### 解法一
排序：

```java
class Solution {
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {
        int[][] ans=new int[R*C][2];
        int idx=0;
        for(int i=0;i<R;i++){
            for(int j=0;j<C;j++){
                ans[idx++]=new int[]{i,j};
            }
        }
        Arrays.sort(ans,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                return Math.abs(a[0]-r0)+Math.abs(a[1]-c0)-Math.abs(b[0]-r0)-Math.abs(b[1]-c0);
            }
        });
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(RClog(RC))$，存储所有点时间复杂度 $O(RC)$，排序时间复杂度 $O(RClog(RC))$。
- 空间复杂度：$O(log(RC))$，即为排序需要使用的栈空间，不考虑返回值的空间占用。

### 解法二
`BFS`：(时间复杂度比排序要高)
```java
class Solution {
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {
        int[][] ans=new int[R*C][2];
        boolean[][] visited=new boolean[R][C];
        Queue<int[]> queue=new LinkedList<>();
        int idx=0;
        int[][] dirs={{1,0},{-1,0},{0,1},{0,-1}};
        queue.offer(new int[]{r0,c0});
        visited[r0][c0]=true;
        while(!queue.isEmpty()){
            int[] temp=queue.poll();
            ans[idx++]=temp;
            int i=temp[0],j=temp[1];
            for(int k=0;k<4;k++){
                int newI=i+dirs[k][0],newJ=j+dirs[k][1];
                if(newI>=0&&newI<R&&newJ>=0&&newJ<C&&!visited[newI][newJ]){
                    visited[newI][newJ]=true;
                    queue.offer(new int[]{newI,newJ});
                }
            }
        }
        return ans;
    }
}
```
