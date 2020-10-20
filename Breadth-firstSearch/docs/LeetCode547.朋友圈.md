# [LeetCode547.朋友圈](https://leetcode-cn.com/problems/friend-circles/)
## 题目描述
班上有 `N` 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 `A` 是 `B` 的朋友，`B` 是 `C` 的朋友，那么我们可以认为 `A` 也是 `C` 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 `N * N` 的矩阵 `M`，表示班级中学生之间的朋友关系。如果`M[i][j] = 1`，表示已知第 `i` 个和 `j` 个学生互为朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。

- `1 <= N <= 200`
- `M[i][i] == 1`
- `M[i][j] == M[j][i]`

### 示例
```
输入：
[[1,1,0],
 [1,1,0],
 [0,0,1]]
输出：2 
解释：已知学生 0 和学生 1 互为朋友，他们在一个朋友圈。
第2个学生自己在一个朋友圈。所以返回 2 。
```
```
输入：
[[1,1,0],
 [1,1,1],
 [0,1,1]]
输出：1
解释：已知学生 0 和学生 1 互为朋友，学生 1 和学生 2 互为朋友，所以学生 0 和学生 2 也是朋友，所以他们三个在一个朋友圈，返回 1 。
```
## 题解
### 解法一
另一种统计图中连通块数量的方法是使用并查集。方法很简单。

使用一个大小为 `N` 的 `parent` 数组，遍历这个图，每个节点我们都遍历所有相邻点，并让相邻点指向它，并设置成一个由 `parent` 节点决定的单独组。这个过程被称为 `union`。这样每个组都有一个唯一的 `parent` 节点，这些节点的父亲为自身。

对于每对新节点，我们找寻他们的父亲。如果父亲节点一样，那么什么都不做他们已经是一个组里。如果父亲不同，说明他们仍然需要合并。因此，将他们的父亲合并，也就是 `parent[parent[x]]=parent[y]`，这样就让他们在一个组里了。

```java
class Solution {
    public int findCircleNum(int[][] M) {
        int N=M.length;
        int[] parent=new int[N];
        for(int i=0;i<N;i++){
            parent[i]=i;
        }
        for(int i=0;i<N;i++){
            for(int j=0;j<i;j++){
                if(M[i][j]==1){
                    union(parent,i,j);
                }
            }
        }
        int cnt=0;
        for(int i=0;i<N;i++){
            if(parent[i]==i){
                cnt++;
            }
        }
        return cnt;
    }
    public int find(int[] parent,int x){
        int a=x;
        while(parent[x]!=x){
            x=parent[x];
        }
        while(a!=parent[a]){//路径压缩
            int temp=parent[a];
            parent[a]=x;
            a=temp;
        }
        return x;
    }
    public void union(int[] parent,int x,int y){
        int parentX=find(parent,x);
        int parentY=find(parent,y);
        if(parentX!=parentY){
            parent[parentX]=parentY;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n^3)$，访问整个矩阵一次，并查集操作需要最坏 $O(n)$ 的时间。
- 空间复杂度：$O(n)$，`parent` 大小为 `n`。
### 解法二
`DFS`:
给定的矩阵可以看成图的邻接矩阵。这样我们的问题可以变成无向图连通块的个数。为了方便理解，考虑如下矩阵：
```
M= [1 1 0 0 0 0
    1 1 0 0 0 0
    0 0 1 1 1 0
    0 0 1 1 0 0
    0 0 1 0 1 0
    0 0 0 0 0 1]
```

如果我们把 `M` 看成图的邻接矩阵，则图为：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201020151719.png)

在这个图中，点的编号表示矩阵 `M` 的下标，`i` 和 `j` 之间有一条边当且仅当 `M[i][j]` 为 1。

为了找到连通块的个数，一个简单的方法就是使用深度优先搜索，从每个节点开始，我们使用一个大小为 `N` 的 `visited` 数组（`M` 大小为 `N×N`），这样 `visited[i]` 表示第 `i` 个元素是否被深度优先搜索访问过。

我们首先选择一个节点，访问任一相邻的节点。然后再访问这一节点的任一相邻节点。这样不断遍历到没有未访问的相邻节点时，回溯到之前的节点进行访问。

```java

public class Solution {
    public int findCircleNum(int[][] M) {
        int[] visited = new int[M.length];
        int count = 0;
        for (int i = 0; i < M.length; i++) {
            if (visited[i] == 0) {
                dfs(M, visited, i);
                count++;
            }
        }
        return count;
    }
    public void dfs(int[][] M, int[] visited, int i) {
        for (int j = 0; j < M.length; j++) {
            if (M[i][j] == 1 && visited[j] == 0) {
                visited[j] = 1;
                dfs(M, visited, j);
            }
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n^2)$，整个矩阵都要被遍历，大小为 `n^2`。
- 空间复杂度：$O(n)$，`visited` 数组的大小。
### 解法三
上面的算法中提到，如果我们把矩阵看成图的邻接矩阵，我们可以使用图算法很快的算出连通块的个数。这可以用到图中的广度优先搜索。

在广度优先搜索中，我们从一个特定点开始，访问所有邻接的节点。然后对于这些邻接节点，我们依然通过访问邻接节点的方式，知道访问所有可以到达的节点。因此，我们按照一层一层的方式访问节点。

我们从任一个节点开始广搜，使用 `visited` 数组记录是否被访问过。增加 `count` 变量当一个连通块已经访问完但是还有节点没有被访问的时候。
```java
public class Solution {
    public int findCircleNum(int[][] M) {
        int[] visited = new int[M.length];
        int count = 0;
        Queue<Integer>queue = new LinkedList<>();
        for (int i = 0; i < M.length; i++) {
            if (visited[i] == 0) {
                queue.offer(i);
                while (!queue.isEmpty()) {
                    int s = queue.poll();
                    visited[s] = 1;
                    for (int j = 0; j < M.length; j++) {
                        if (M[s][j] == 1 && visited[j] == 0)
                            queue.offer(j);
                    }
                }
                count++;
            }
        }
        return count;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n^2)$，整个矩阵都要被访问。
- 空间复杂度：$O(n)$，`queue` 和 `visited` 数组的大小。


