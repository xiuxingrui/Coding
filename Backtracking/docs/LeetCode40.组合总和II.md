# [LeetCode40.组合总和II](https://leetcode-cn.com/problems/combination-sum-ii/)
## 题目描述
给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

说明：

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

### 示例
```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```
## 题解
```java
public class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int len = candidates.length;
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) {
            return res;
        }
        // 先将数组排序，这一步很关键
        Arrays.sort(candidates);
        List<Integer> path = new ArrayList<>();
        dfs(candidates,target,len,0,0, path, res);
        return res;
    }
    public void dfs(int[] candidates,int target, int len, int begin, int cur, List<Integer> path, List<List<Integer>> res) {
        if (cur == target) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = begin; i < len; i++) {
            // 大剪枝
            if (cur+candidates[i]>target) {
                break;
            }
            // 小剪枝
            if (i > begin && candidates[i] == candidates[i - 1]) {
                continue;
            }
            path.add(candidates[i]);
            // 因为元素不可以重复使用，这里递归传递下去的是 i + 1 而不是 i
            dfs(candidates,target,len, i + 1, cur+candidates[i], path, res);
            path.remove(path.size()-1);
        }
    }
}
```
