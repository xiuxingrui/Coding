# [LeetCode279.完全平方数](https://leetcode-cn.com/problems/perfect-squares/)
## 题目描述
给定正整数 `n`，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 `n`。你需要让组成和的完全平方数的个数最少。

### 示例
```
输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
```
```
输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```
## 题解
### 解法一:动态规划
- 首先初始化长度为`n+1`的数组`dp`，每个位置都为0
- 如果`n`为0，则结果为0
- 对数组进行遍历，下标为`i`，每次都将当前数字先更新为最大的结果，即`dp[i]=i`，比如i=4，最坏结果为4=1+1+1+1即为4个数字
- 动态转移方程为：`dp[i] = Math.min(dp[i], dp[i - j * j] + 1)`，`i`表示当前数字，`j*j`表示平方数
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1]; // 默认初始化值都为0
        for (int i = 1; i <= n; i++) {
            dp[i] = i; // 最坏的情况就是每次+1
            for (int j = 1; i - j * j >= 0; j++) { 
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1); // 动态转移方程
            }
        }
        return dp[n];
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n \cdot \sqrt{n})$，在主步骤中，我们有一个嵌套循环，其中外部循环是 `n` 次迭代，而内部循环最多需要 $\sqrt{n}$迭代。
- 空间复杂度：$(n)$，使用了一个一维数组 `dp`。
### 解法二:BFS
相对于解法一的 `DFS` ，当然也可以使用 `BFS` 。

`DFS` 是一直做减法，然后一直减一直减，直到减到 0 算作找到一个解。属于一个解一个解的寻找。

`BFS` 的话，我们可以一层一层的算。第一层依次减去一个平方数得到第二层，第二层依次减去一个平方数得到第三层。直到某一层出现了 0，此时的层数就是我们要找到平方数和的最小个数。

举个例子，`n = 12`，每层的话每个节点依次减 1, 4, 9...。如下图，灰色表示当前层重复的节点，不需要处理。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200819222647.png)

如上图，当出现 0 的时候遍历就可以停止，此时是第 3 层（从 0 计数），所以最终答案就是 3。

实现的话当然离不开队列，此外我们需要一个 `set` 来记录重复的解。

```java
class Solution {
    public int numSquares(int n) {
        Queue<Integer> queue = new LinkedList<>();
        HashSet<Integer> visited = new HashSet<>();
        int level = 0;
        queue.add(n);
        while (!queue.isEmpty()) {
            int size = queue.size();
            level++; // 开始生成下一层
            for (int i = 0; i < size; i++) {
                int cur = queue.poll();
                //依次减 1, 4, 9... 生成下一层的节点
                for (int j = 1; j * j <= cur; j++) {
                    int next = cur - j * j;
                    if (next == 0) {
                        return level;
                    }
                    if (!visited.contains(next)) {
                        queue.offer(next);
                        visited.add(next);
                    }
                }
            }
        }
        return -1;
    }
}
```
### 解法三：回溯
相当于一种暴力的方法，去考虑所有的分解方案，找出最小的解，举个例子。
```
n = 12
先把 n 减去一个平方数，然后求剩下的数分解成平方数和所需的最小个数

把 n 减去 1, 然后求出 11 分解成平方数和所需的最小个数,记做 n1
那么当前方案总共需要 n1 + 1 个平方数

把 n 减去 4, 然后求出 8 分解成平方数和所需的最小个数,记做 n2
那么当前方案总共需要 n2 + 1 个平方数

把 n 减去 9, 然后求出 3 分解成平方数和所需的最小个数,记做 n3
那么当前方案总共需要 n3 + 1 个平方数

下一个平方数是 16, 大于 12, 不能再分了。

接下来我们只需要从 (n1 + 1), (n2 + 1), (n3 + 1) 三种方案中选择最小的个数, 
此时就是 12 分解成平方数和所需的最小个数了

至于求 11、8、3 分解成最小平方数和所需的最小个数继续用上边的方法去求

直到如果求 0 分解成最小平方数的和的个数, 返回 0 即可
```
```java
class Solution {
    public int numSquares(int n) {
        return numSquaresHelper(n);
    }
    private int numSquaresHelper(int n) {
        if (n == 0) {
            return 0;
        }
        int count = Integer.MAX_VALUE;
        //依次减去一个平方数
        for (int i = 1; i * i <= n; i++) {
            //选最小的
            count = Math.min(count, numSquaresHelper(n - i * i) + 1);
        }
        return count;
    }
}
```
当然上边的会造成超时，很多解会重复的计算，之前也遇到很多这种情况了。我们需要 `memoization` 技术，也就是把过程中的解利用 `HashMap` 全部保存起来即可。
```java
class Solution {
    public int numSquares(int n) {
        return numSquaresHelper(n, new HashMap<Integer, Integer>());
    }

    private int numSquaresHelper(int n, HashMap<Integer, Integer> map) {
        if (map.containsKey(n)) {
            return map.get(n);
        }
        if (n == 0) {
            return 0;
        }
        int count = Integer.MAX_VALUE;
        for (int i = 1; i * i <= n; i++) {
            count = Math.min(count, numSquaresHelper(n - i * i, map) + 1);
        }
        map.put(n, count);
        return count;
    }
}
```
### 解法四:暴力
自己的解法，超时
```java
class Solution {
    int minCnt=Integer.MAX_VALUE;
    public int numSquares(int n) {
        if(n==1){
            return 1;
        }
        int s=(int)Math.sqrt(n);
        numSquaresHelper(n,s,0,0);
        return minCnt;
    }
    public void numSquaresHelper(int n,int s,int sum,int cnt){
        if(sum==n){
            if(cnt<minCnt){
                minCnt=cnt;
            }
            return;
        }
        for(int num=1;num<=s;num++){
            if(sum+num*num<=n){
                numSquaresHelper(n,s,sum+num*num,cnt+1);
            }
        }
    }
}
```

