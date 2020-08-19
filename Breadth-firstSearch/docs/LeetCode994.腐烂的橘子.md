# [LeetCode994.腐烂的橘子](https://leetcode-cn.com/problems/rotting-oranges/)
## 题目描述
在给定的网格中，每个单元格可以有以下三个值之一：

- 值 0 代表空单元格；
- 值 1 代表新鲜橘子；
- 值 2 代表腐烂的橘子。

每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1。

- `1 <= grid.length <= 10`
- `1 <= grid[0].length <= 10`
- `grid[i][j] 仅为 0、1 或 2`
### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200819193246.png)
```
输入：[[2,1,1],[1,1,0],[0,1,1]]
输出：4
```
```
输入：[[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。
```
```
输入：[[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```
## 题解
`BFS` 可以看成是层序遍历。从某个结点出发，`BFS` 首先遍历到距离为 1 的结点，然后是距离为 2、3、4…… 的结点。因此，`BFS` 可以用来求最短路径问题。`BFS` 先搜索到的结点，一定是距离最近的结点。

再看看这道题的题目要求：返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。翻译一下，实际上就是求腐烂橘子到所有新鲜橘子的最短路径。那么这道题使用 `BFS`，应该是毫无疑问的了。

我们都知道 `BFS` 需要使用队列，代码框架是这样子的（伪代码）：
```Python
while queue 非空:
	node = queue.pop()
    for node 的所有相邻结点 m:
        if m 未访问过:
            queue.push(m)
```
但是用 `BFS` 来求最短路径的话，这个队列中第 1 层和第 2 层的结点会紧挨在一起，无法区分。因此，我们需要稍微修改一下代码，在每一层遍历开始前，记录队列中的结点数量 `n` ，然后一口气处理完这一层的 `n` 个结点。代码框架是这样的：

```Python
depth = 0 # 记录遍历到第几层
while queue 非空:
    depth++
    n = queue 中的元素个数
    循环 n 次:
        node = queue.pop()
        for node 的所有相邻结点 m:
            if m 未访问过:
                queue.push(m)
```
有了计算最短路径的层序 `BFS` 代码框架，写这道题就很简单了。这道题的主要思路是：

- 一开始，我们找出所有腐烂的橘子，将它们放入队列，作为第 0 层的结点。
- 然后进行 `BFS` 遍历，每个结点的相邻结点可能是上、下、左、右四个方向的结点，注意判断结点位于网格边界的特殊情况。
- 由于可能存在无法被污染的橘子，我们需要记录新鲜橘子的数量。在 `BFS` 中，每遍历到一个橘子（污染了一个橘子），就将新鲜橘子的数量减一。如果 BFS 结束后这个数量仍未减为零，说明存在无法被污染的橘子。

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        if(grid==null||grid.length==0||grid[0].length==0){
            return -1;
        }
        int m=grid.length;
        int n=grid[0].length;
        int oneCount=0;
        Queue<int[]> queue= new LinkedList<>();
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1){
                    oneCount++;
                }
                if(grid[i][j]==2){
                    int[] pair=new int[]{i,j};
                    queue.offer(pair);
                }
            }
        }
        int minMinute=0;
        while(oneCount>0&&!queue.isEmpty()){
            minMinute++;
            int size=queue.size();
            for(int k=0;k<size;k++){
                int[] temp=queue.poll();
                int i=temp[0];
                int j=temp[1];
                if(i-1>=0&&grid[i-1][j]==1){
                    grid[i-1][j]=2;
                    oneCount--;
                    int[] pair=new int[]{i-1,j};
                    queue.offer(pair);
                }
                if(i+1<m&&grid[i+1][j]==1){
                    grid[i+1][j]=2;
                    oneCount--;
                    int[] pair=new int[]{i+1,j};
                    queue.offer(pair);
                }
                if(j-1>=0&&grid[i][j-1]==1){
                    grid[i][j-1]=2;
                    oneCount--;
                    int[] pair=new int[]{i,j-1};
                    queue.offer(pair);
                }
                if(j+1<n&&grid[i][j+1]==1){
                    grid[i][j+1]=2;
                    oneCount--;
                    int[] pair=new int[]{i,j+1};
                    queue.offer(pair);
                }
            }
        }
        if(oneCount==0){
            return minMinute;
        }else{
            return -1;
        }
    }
}
```
自己的写法:
```java
class Solution {
    public int orangesRotting(int[][] grid) {
        if(grid==null||grid.length==0||grid[0].length==0){
            return -1;
        }
        int m=grid.length;
        int n=grid[0].length;
        int oneCount=0;
        Queue<Pair> queue= new LinkedList<>();
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1){
                    oneCount++;
                }
                if(grid[i][j]==2){
                    Pair<Integer,Integer> pair=new Pair<>(i,j);
                    queue.offer(pair);
                }
            }
        }
        if(oneCount==0){
            return 0;
        }
        int minMinute=0;
        while(!queue.isEmpty()){
            minMinute++;
            int size=queue.size();
            for(int k=0;k<size;k++){
                Pair<Integer,Integer> temp=queue.poll();
                int i=temp.getKey();
                int j=temp.getValue();
                if(i-1>=0&&grid[i-1][j]==1){
                    grid[i-1][j]=2;
                    oneCount--;
                    Pair<Integer,Integer> pair=new Pair(i-1,j);
                    queue.offer(pair);
                }
                if(i+1<m&&grid[i+1][j]==1){
                    grid[i+1][j]=2;
                    oneCount--;
                    Pair<Integer,Integer> pair=new Pair(i+1,j);
                    queue.offer(pair);
                }
                if(j-1>=0&&grid[i][j-1]==1){
                    grid[i][j-1]=2;
                    oneCount--;
                    Pair<Integer,Integer> pair=new Pair(i,j-1);
                    queue.offer(pair);
                }
                if(j+1<n&&grid[i][j+1]==1){
                    grid[i][j+1]=2;
                    oneCount--;
                    Pair<Integer,Integer> pair=new Pair(i,j+1);
                    queue.offer(pair);
                }
                if(oneCount==0){
                    return minMinute;
                }
            }
        }
        return -1;
    }
}
```
