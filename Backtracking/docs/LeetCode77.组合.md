# [LeetCode77.组合](https://leetcode-cn.com/problems/combinations/)
## 题目描述
给定两个整数 `n` 和 `k`，返回 `1 ... n` 中所有可能的 `k` 个数的组合。
### 示例
```
输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```
## 题解
```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        getAns(1,n, k, new ArrayList<Integer>(), ans);
        return ans;
}

    private void getAns(int start, int n, int k, ArrayList<Integer> temp,List<List<Integer>> ans) {
        //如果 temp 里的数字够了 k 个，就把它加入到结果中
        if(temp.size() == k){
            ans.add(new ArrayList<Integer>(temp));
            return;
        }
        //从 start 到 n
        for (int i = start; i <= n; i++) {
            //将当前数字加入 temp
            temp.add(i);
            //进入递归
            getAns(i+1, n, k, temp, ans);
            //将当前数字删除，进入下次 for 循环
            temp.remove(temp.size() - 1);
        }
    }
}
```
#### 复杂度分析
- 时间复杂度:$O\left(k C_{N}^{k}\right)$,其中$C_{N}^{k}=\frac{N !}{(N-k) ! k !}$是要构成的组合数。
  - `append` / `pop` (`add` / `removeLast`) 操作使用常数时间，唯一耗费时间的是将长度为 `k` 的组合添加到输出中。
- 空间复杂度 : $O\left(C_{N}^{k}\right)$，用于保存全部组合数以输出。