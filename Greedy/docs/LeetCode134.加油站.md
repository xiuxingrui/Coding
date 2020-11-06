# [LeetCode134.加油站](https://leetcode-cn.com/problems/gas-station/)
## 题目描述
在一条环路上有 `N` 个加油站，其中第 `i` 个加油站有汽油 `gas[i]` 升。

你有一辆油箱容量无限的的汽车，从第 `i` 个加油站开往第 `i+1` 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。

如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

说明: 

- 如果题目有解，该答案即为唯一答案。
- 输入数组均为非空数组，且长度相同。
- 输入数组中的元素均为非负数。

### 示例
```
输入: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

输出: 3

解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
```
```
输入: 
gas  = [2,3,4]
cost = [3,4,3]

输出: -1

解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。
```
## 题解
### 解法一
- 初始化 `total_tank` 和 `curr_tank` 为 0 ，并且选择 0 号加油站为起点。
- 遍历所有的加油站：
  - 每一步中，都通过加上 `gas[i]` 和减去 `cost[i]` 来更新 `total_tank` 和 `curr_tank` 。
  - 如果在 `i + 1` 号加油站， `curr_tank < 0` ，将 `i + 1` 号加油站作为新的起点，同时重置 `curr_tank = 0` ，让油箱也清空。
- 如果 `total_tank < 0` ，返回 -1 ，否则返回 `starting station`。

问题1: 为什么应该将起始站点设为`k+1`？

因为`k->k+1`站耗油太大，`0->k`站剩余油量都是不为负的，每减少一站，就少了一些剩余油量。所以如果从`k`前面的站点作为起始站，剩余油量不可能冲过`k+1`站。

问题2: 为什么如果`k+1->end`全部可以正常通行，且`rest>=0`就可以说明车子从`k+1`站点出发可以开完全程？

因为，起始点将当前路径分为`A`、`B`两部分。其中，必然有(1)`A`部分剩余油量<0。(2)`B`部分剩余油量>0。

所以，无论多少个站，都可以抽象为两个站点（`A`、`B`）。(1)从`B`站加满油出发，(2)开往`A`站，车加油，(3)再开回`B`站的过程。

重点：`B`剩余的油>=`A`缺少的总油。必然可以推出，`B`剩余的油>=`A`站点的每个子站点缺少的油。

```java
class Solution {
  public int canCompleteCircuit(int[] gas, int[] cost) {
    int n = gas.length;

    int total_tank = 0;
    int curr_tank = 0;
    int starting_station = 0;
    for (int i = 0; i < n; ++i) {
      total_tank += gas[i] - cost[i];
      curr_tank += gas[i] - cost[i];
      // If one couldn't get here,
      if (curr_tank < 0) {
        // Pick up the next station as the starting one.
        starting_station = i + 1;
        // Start with an empty tank.
        curr_tank = 0;
      }
    }
    return total_tank >= 0 ? starting_station : -1;
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$ ， 这是因为只有一个遍历了所有加油站一次的循环。
- 空间复杂度：$O(1)$ ，因为此算法只使用了常数个变量。

### 解法二
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int len=gas.length;
        for(int i=0;i<len;i++){
            int j=i;
            int cur=0;
            boolean first=true,flag=true;
            while(first||j!=i){
                first=false;
                cur+=gas[j];
                if(cur<cost[j]){
                    flag=false;
                    break;
                }
                cur-=cost[j];
                j=(j+1)%len;
            }
            if(flag&&j==i){
                return i;
            }
        }
        return -1;
    }
}
```
或
```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    int n = gas.length;
    //考虑从每一个点出发
    for (int i = 0; i < n; i++) {
        int j = i;
        int remain = gas[i];
        //当前剩余的油能否到达下一个点
        while (remain - cost[j] >= 0) {
            //减去花费的加上新的点的补给
            remain = remain - cost[j] + gas[(j + 1) % n];
            j = (j + 1) % n;
            //j 回到了 i
            if (j == i) {
                return i;
            }
        }
    }
    //任何点都不可以
    return -1;
}
```