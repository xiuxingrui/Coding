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
### 解法一
```java
public class Solution {

    /**
     * @param candidates 候选数组
     * @param len
     * @param begin      从候选数组的 begin 位置开始搜索
     * @param residue    表示剩余，这个值一开始等于 target，基于题目中说明的"所有数字（包括目标数）都是正整数"这个条件
     * @param path       从根结点到叶子结点的路径
     * @param res
     */
    private void dfs(int[] candidates, int len, int begin, int residue, Deque<Integer> path, List<List<Integer>> res) {
        if (residue == 0) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = begin; i < len; i++) {
            // 大剪枝
            if (residue - candidates[i] < 0) {
                break;
            }

            // 小剪枝
            if (i > begin && candidates[i] == candidates[i - 1]) {
                continue;
            }

            path.addLast(candidates[i]);

            // 因为元素不可以重复使用，这里递归传递下去的是 i + 1 而不是 i
            dfs(candidates, len, i + 1, residue - candidates[i], path, res);

            path.removeLast();
        }
    }

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int len = candidates.length;
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) {
            return res;
        }

        // 先将数组排序，这一步很关键
        Arrays.sort(candidates);

        Deque<Integer> path = new ArrayDeque<>(len);
        dfs(candidates, len, 0, target, path, res);
        return res;
    }
}
```
### 解法二
```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        int len=candidates.length;
        Arrays.sort(candidates);
        boolean[] used=new boolean[len];
        helper(res,path,target,candidates,len,used);
        return res;
    }
    public void helper(List<List<Integer>> res,LinkedList<Integer> path,int target,int[] nums,int len,boolean[] used){
        if(target<=0) return;
        for(int i=0;i<len;++i){
            if(used[i]) continue;
            if(i>0&&nums[i]==nums[i-1]&&!used[i-1]) continue;
            if(path.size()==0||(path.size()!=0)&&nums[i]>=path.getLast()){
                used[i]=true;
                path.addLast(nums[i]);
                target-=nums[i];
                if(target==0){
                    res.add(new LinkedList<>(path));
                }
                helper(res,path,target,nums,len,used);
                path.removeLast();
                target+=nums[i];
                used[i]=false;
            }
        }
    }
}
```
