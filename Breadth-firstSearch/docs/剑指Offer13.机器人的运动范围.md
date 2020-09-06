# [剑指Offer13.机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)
## 题目描述
地上有一个m行n列的方格，从坐标 `[0,0]` 到坐标 `[m-1,n-1]` 。一个机器人从坐标 `[0, 0]` 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于`k`的格子。例如，当`k`为18时，机器人能够进入方格 `[35, 37]` ，因为3+5+3+7=18。但它不能进入方格 `[35, 38]`，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

- `1 <= n,m <= 100`
- `0 <= k <= 20`
### 示例
```
输入：m = 2, n = 3, k = 1
输出：3
```
```
输入：m = 3, n = 1, k = 0
输出：1
```
## 题解
### 解法一:BFS
根据可达解的结构，易推出机器人可 仅通过向右和向下移动，访问所有可达解 。

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited=new boolean[m][n];
        Queue<int[]> queue=new LinkedList<>();
        int ans=0;
        if(canGo(0,0,m,n,k,visited)==true){
            visited[0][0]=true;
            queue.offer(new int[]{0,0});
        }
        while(!queue.isEmpty()){
            int[] temp=queue.poll();
            ans++;
            int tempX=temp[0];
            int tempY=temp[1];
            // if(canGo(tempX-1,tempY,m,n,k,visited)==true){
            //     visited[tempX-1][tempY]=true;
            //     queue.offer(new int[]{tempX-1,tempY});
            // }
            if(canGo(tempX+1,tempY,m,n,k,visited)==true){
                visited[tempX+1][tempY]=true;
                 queue.offer(new int[]{tempX+1,tempY});
            }
            // if(canGo(tempX,tempY-1,m,n,k,visited)==true){
            //     visited[tempX][tempY-1]=true;
            //      queue.offer(new int[]{tempX,tempY-1});
            // }
            if(canGo(tempX,tempY+1,m,n,k,visited)==true){
                visited[tempX][tempY+1]=true;
                 queue.offer(new int[]{tempX,tempY+1});
            }
        }
        return ans;
    }
    public boolean canGo(int x,int y,int m,int n,int k,boolean[][] visited){
        if(x<0||x>m-1||y<0||y>n-1||visited[x][y]){
            return false;
        }
        int sumX=0,sumY=0;
        while(x!=0){
            sumX+=x%10;
            x/=10;
        }
        while(y!=0){
            sumY+=y%10;
            y/=10;
        }
        return sumX+sumY<=k;
    }
}
```
```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited=new boolean[m][n];
        Queue<Pair<Integer,Integer>> queue=new LinkedList<>();
        int ans=0;
        if(canGo(0,0,m,n,k,visited)==true){
            visited[0][0]=true;
            queue.offer(new Pair<Integer,Integer>(0,0));
        }
        while(!queue.isEmpty()){
            Pair<Integer,Integer> temp=queue.poll();
            ans++;
            int tempX=temp.getKey();
            int tempY=temp.getValue();
            Pair<Integer,Integer> tempPair;
            // if(canGo(tempX-1,tempY,m,n,k,visited)==true){
            //     visited[tempX-1][tempY]=true;
            //     tempPair=new Pair(tempX-1,tempY);
            //     queue.offer(tempPair);
            // }
            if(canGo(tempX+1,tempY,m,n,k,visited)==true){
                visited[tempX+1][tempY]=true;
                tempPair=new Pair(tempX+1,tempY);
                queue.offer(tempPair);
            }
            // if(canGo(tempX,tempY-1,m,n,k,visited)==true){
            //     visited[tempX][tempY-1]=true;
            //     tempPair=new Pair(tempX,tempY-1);
            //     queue.offer(tempPair);
            // }
            if(canGo(tempX,tempY+1,m,n,k,visited)==true){
                visited[tempX][tempY+1]=true;
                tempPair=new Pair(tempX,tempY+1);
                queue.offer(tempPair);
            }
        }
        return ans;
    }
    public boolean canGo(int x,int y,int m,int n,int k,boolean[][] visited){
        if(x<0||x>m-1||y<0||y>n-1||visited[x][y]){
            return false;
        }
        int sumX=0,sumY=0;
        while(x!=0){
            sumX+=x%10;
            x/=10;
        }
        while(y!=0){
            sumY+=y%10;
            y/=10;
        }
        return sumX+sumY<=k;
    }
}
```

### 解法二:DFS
```java
class Solution {
    public int movingCount(int m, int n, int k) {
    //临时变量visited记录格子是否被访问过
    boolean[][] visited = new boolean[m][n];
    return dfs(0, 0, m, n, k, visited);
    }

    public int dfs(int i, int j, int m, int n, int k, boolean[][] visited) {
        //i >= m || j >= n是边界条件的判断，k < sum(i, j)判断当前格子坐标是否
        // 满足条件，visited[i][j]判断这个格子是否被访问过
        if (i >= m || j >= n || k < sum(i, j) || visited[i][j])
            return 0;
        //标注这个格子被访问过
        visited[i][j] = true;
        //沿着当前格子的右边和下边继续访问
        return 1 + dfs(i + 1, j, m, n, k, visited) + dfs(i, j + 1, m, n, k, visited);
    }

    //计算两个坐标数字的和
    private int sum(int i, int j) {
        int sum = 0;
        while (i != 0) {
            sum += i % 10;
            i /= 10;
        }
        while (j != 0) {
            sum += j % 10;
            j /= 10;
        }
        return sum;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mn)$，其中 `m` 为方格的行数， `n` 为方格的列数。一共有 $O(mn)$ 个状态需要计算，每个状态递推计算的时间复杂度为 $O(1)$，所以总时间复杂度为 $O(mn)$。
- 空间复杂度：$O(mn)$，其中 `m`为方格的行数，`n` 为方格的列数。我们需要 $O(mn)$ 大小的结构来记录每个位置是否可达。
