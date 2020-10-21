# [LeetCode207.课程表](https://leetcode-cn.com/problems/course-schedule/)
## 题目描述
你这个学期必须选修 `numCourse` 门课程，记为 0 到 `numCourse-1` 。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：`[0,1]`

给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

- 输入的先决条件是由 边缘列表 表示的图形，而不是 邻接矩阵 。详情请参见图的表示法。
- 你可以假定输入的先决条件中没有重复的边。
- `1 <= numCourses <= 10^5`

### 示例
```
输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
```
```
输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
```
## 题解
### 解法一
- 本题可约化为： 课程安排图是否是 有向无环图(`DAG`)。即课程间规定了前置条件，但不能构成任何环路，否则课程前置条件将不成立。
- 思路是通过 拓扑排序 判断此课程安排图是否是 有向无环图(`DAG`) 。 拓扑排序原理： 对 `DAG` 的顶点进行排序，使得对每一条有向边 `(u,v)`，均有 `u`（在排序记录中）比 `v` 先出现。亦可理解为对某点 `v` 而言，只有当 `v` 的所有源点均出现了，`v` 才能出现。
- 通过课程前置条件列表 `prerequisites` 可以得到课程安排图的 邻接表 `graph`，以降低算法时间复杂度

- 统计课程安排图中每个节点的入度，生成 入度表 `indegrees`。
- 借助一个队列 `queue`，将所有入度为 0 的节点入队。
- 当 `queue` 非空时，依次将队首节点出队，在课程安排图中删除此节点 `pre`：
- 并不是真正从邻接表中删除此节点 `pre`，而是将此节点对应所有邻接节点 `cur` 的入度 −1，即 `indegrees[cur] -= 1`。
- 当入度 −1后邻接节点 `cur` 的入度为 0，说明 `cur` 所有的前驱节点已经被 “删除”，此时将 `cur` 入队。
- 在每次 `pre` 出队时，执行 `numCourses--`；
若整个课程安排图是有向无环图（即可以安排），则所有节点一定都入队并出队过，即完成拓扑排序。换个角度说，若课程安排图中存在环，一定有节点的入度始终不为 0。

因此，拓扑排序出队次数等于课程个数，返回 `numCourses == 0` 判断课程是否可以成功安排。
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indegrees=new int[numCourses];
        List<List<Integer>> graph=new ArrayList<>();
        Queue<Integer> queue=new LinkedList<>();
        for(int i=0;i<numCourses;i++){
            graph.add(new ArrayList<>());
        }
        for(int[] cp:prerequisites){
            indegrees[cp[0]]++;
            graph.get(cp[1]).add(cp[0]);
        }
        for(int i=0;i<numCourses;i++){
            if(indegrees[i]==0){
                queue.offer(i);
            }
        }
        while(!queue.isEmpty()){
            int pre=queue.poll();
            numCourses--;
            for(int cur:graph.get(pre)){
                if(--indegrees[cur]==0){
                    queue.offer(cur);
                }
            }
        }
        return numCourses==0;
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(N+M)$： 遍历一个图需要访问所有节点和所有临边，`N` 和 `M` 分别为节点数量和临边数量；
- 空间复杂度 $O(N+M)$： 为建立邻接表所需额外空间，`graph` 长度为 `N` ，并存储 `M` 条临边的数据。
### 解法二
原理是通过 `DFS` 判断图中是否有环。

算法流程：

借助一个标志列表 `flags`，用于判断每个节点 `i` （课程）的状态：
- 未被 `DFS` 访问：`i == 0`；
- 已被其他节点启动的 `DFS` 访问：`i == -1`；
- 已被当前节点启动的 `DFS` 访问：`i == 1`。

对 `numCourses` 个节点依次执行 `DFS`，判断每个节点起步 `DFS` 是否存在环，若存在环直接返回 `False`。

`DFS` 流程；

终止条件：

- 当 `flag[i] == -1`，说明当前访问节点已被其他节点启动的 `DFS` 访问，无需再重复搜索，直接返回 `True`。
- 当 `flag[i] == 1`，说明在本轮 `DFS` 搜索中节点 `i` 被第 2 次访问，即 课程安排图有环 ，直接返回 `False`。

将当前访问节点 `i` 对应 `flag[i]` 置 1，即标记其被本轮 `DFS` 访问过；

递归访问当前节点 `i` 的所有邻接节点 `j`，当发现环直接返回 `False`；

当前节点所有邻接节点已被遍历，并没有发现环，则将当前节点 `flag` 置为 −1 并返回 `True`。

若整个图 `DFS` 结束并未发现环，返回 `True`。

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adjacency = new ArrayList<>();
        for(int i = 0; i < numCourses; i++)
            adjacency.add(new ArrayList<>());
        int[] flags = new int[numCourses];
        for(int[] cp : prerequisites)
            adjacency.get(cp[1]).add(cp[0]);
        for(int i = 0; i < numCourses; i++)
            if(!dfs(adjacency, flags, i)) return false;
        return true;
    }
    private boolean dfs(List<List<Integer>> adjacency, int[] flags, int i) {
        if(flags[i] == 1) return false;
        if(flags[i] == -1) return true;
        flags[i] = 1;
        for(Integer j : adjacency.get(i))
            if(!dfs(adjacency, flags, j)) return false;
        flags[i] = -1;
        return true;
    }
}
```
#### 复杂度分析：
- 时间复杂度 $O(N+M)$： 遍历一个图需要访问所有节点和所有临边，`N` 和 `M` 分别为节点数量和临边数量；
- 空间复杂度 $O(N+M)$： 为建立邻接表所需额外空间，`adjacency` 长度为 `N` ，并存储 `M` 条临边的数据。
