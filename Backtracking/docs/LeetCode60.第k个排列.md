# [LeetCode60.第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)
## 题目描述
给出集合 `[1,2,3,…,n]`，其所有元素共有 `n!` 种排列。

按大小顺序列出所有排列情况，并一一标记，当 `n = 3` 时, 所有排列如下：

```
"123"
"132"
"213"
"231"
"312"
"321"
```

给定 `n` 和 `k`，返回第 `k` 个排列。

- 给定 `n` 的范围是 `[1, 9]`。
- 给定 `k` 的范围是`[1,  n!]`。

### 示例
```
输入: n = 3, k = 3
输出: "213"
```
```
输入: n = 4, k = 9
输出: "2314"
```
## 题解
### 解法一
思路分析：容易想到，使用同「力扣」第 46 题： 全排列 的回溯搜索算法，依次得到全排列，输出第 `k` 个全排列即可。事实上，我们不必求出所有的全排列。

基于以下几点考虑：

所求排列 一定在叶子结点处得到，进入每一个分支，可以根据已经选定的数的个数，进而计算还未选定的数的个数，然后计算阶乘，就知道这一个分支的 叶子结点 的个数：

- 如果 `k` 大于这一个分支将要产生的叶子结点数，直接跳过这个分支，这个操作叫「剪枝」；
- 如果 `k` 小于等于这一个分支将要产生的叶子结点数，那说明所求的全排列一定在这一个分支将要产生的叶子结点里，需要递归求解。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200903225954.png)

下面以示例 2：输入:`n=4`，`k=9`，介绍如何使用「回溯 + 剪枝」的思想得到输出 "2314"。

编码注意事项：

- 计算阶乘的时候，可以使用循环计算。注意：`0!=1`，它表示了没有数可选的时候，即表示到达叶子结点了，排列数只剩下 1 个；
- 题目中说「给定 `n` 的范围是 `[1,9]`，可以把从 0 到 9 的阶乘计算好，放在一个数组里，可以根据索引直接获得阶乘值；
- 编码的时候，+1 还是 −1 ，大于还是大于等于，这些不能靠猜。常见的做法是：代入一个具体的数值，认真调试。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200905103132.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200905105746.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200905105809.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200905105822.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200905105833.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200905105846.png)

```java
public class Solution {

    /**
     * 记录数字是否使用过
     */
    private boolean[] used;

    /**
     * 阶乘数组
     */
    private int[] factorial;

    private int n;
    private int k;

    public String getPermutation(int n, int k) {
        this.n = n;
        this.k = k;
        calculateFactorial(n);

        // 查找全排列需要的布尔数组
        used = new boolean[n + 1];
        Arrays.fill(used, false);

        StringBuilder path = new StringBuilder();
        dfs(0, path);
        return path.toString();
    }


    /**
     * @param index 在这一步之前已经选择了几个数字，其值恰好等于这一步需要确定的下标位置
     * @param path
     */
    private void dfs(int index, StringBuilder path) {
        if (index == n) {
            return;
        }

        // 计算还未确定的数字的全排列的个数，第 1 次进入的时候是 n - 1
        int cnt = factorial[n - 1 - index];
        for (int i = 1; i <= n; i++) {
            if (used[i]) {
                continue;
            }
            if (cnt < k) {
                k -= cnt;
                continue;
            }
            path.append(i);
            used[i] = true;
            dfs(index + 1, path);
            // 注意 1：没有回溯（状态重置）的必要

            // 注意 2：这里要加 return，后面的数没有必要遍历去尝试了
            return;
        }
    }

    /**
     * 计算阶乘数组
     *
     * @param n
     */
    private void calculateFactorial(int n) {
        factorial = new int[n + 1];
        factorial[0] = 1;
        for (int i = 1; i <= n; i++) {
            factorial[i] = factorial[i - 1] * i;
        }
    }
}
```
### 解法二
自己的解法：
```
class Solution {
    int count=0;
    List<List<Integer>> res=new ArrayList<>();
    public String getPermutation(int n, int k) {
        LinkedList<Integer> list=new LinkedList<>();
        boolean[] used=new boolean[n+1];
        backtrack(0,list,n,k,used);
        List<Integer> ans=res.get(0);
        StringBuilder sb=new StringBuilder();
        for(Integer num:ans){
            sb.append(num.toString());
        }
        return sb.toString();
    }
    public void backtrack(int depth,LinkedList<Integer> list,int n,int k,boolean[] used){
        if(depth==n){
            ++count;
            if(count==k){
                res.add(new LinkedList<>(list));
            }
            return;
        }
        for(int i=1;i<=n;++i){
            if(used[i]==true){
                continue;
            }
            used[i]=true;
            list.addLast(i);
            backtrack(depth+1,list,n,k,used);
            if(res.isEmpty()==false){
                return;
            }
            list.removeLast();
            used[i]=false;
        }
    }
}
```
