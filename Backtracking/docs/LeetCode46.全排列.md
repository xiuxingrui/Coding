# [LeetCode46.全排列](https://leetcode-cn.com/problems/permutations/)
## 题目描述
给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

### 示例
```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## 题解
写法一:
```java
class Solution {
    /* 主函数，输入一组不重复的数字，返回它们的全排列 */
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new LinkedList<>();
        LinkedList<Integer> track = new LinkedList<>();
        boolean[] used=new boolean[nums.length];
        backtrack(res,nums, track,used);
        return res;
    }
    // 路径：记录在 track 中
    // 选择列表：nums 中不存在于 track 的那些元素
    // 结束条件：nums 中的元素全都在 track 中出现
    void backtrack(List<List<Integer>> res,int[] nums, LinkedList<Integer> track,boolean[] used) {
        // 触发结束条件
        if (track.size() == nums.length) {
            res.add(new LinkedList(track));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            // 排除不合法的选择
            if (used[i]==true)
                continue;
            used[i]=true;
            // 做选择
            track.add(nums[i]);
            // 进入下一层决策树
            backtrack(res,nums, track,used);
            // 取消选择
            track.removeLast();
            used[i]=false;
        }
    }
}
```
写法二:
```java
class Solution {
    /* 主函数，输入一组不重复的数字，返回它们的全排列 */
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new LinkedList<>();
        LinkedList<Integer> track = new LinkedList<>();
        backtrack(res,nums, track);
        return res;
    }
    // 路径：记录在 track 中
    // 选择列表：nums 中不存在于 track 的那些元素
    // 结束条件：nums 中的元素全都在 track 中出现
    void backtrack(List<List<Integer>> res,int[] nums, LinkedList<Integer> track) {
        // 触发结束条件
        if (track.size() == nums.length) {
            res.add(new LinkedList(track));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            // 排除不合法的选择
            if (track.contains(nums[i]))
                continue;
            // 做选择
            track.add(nums[i]);
            // 进入下一层决策树
            backtrack(res,nums, track);
            // 取消选择
            track.removeLast();
        }
    }
}
```
