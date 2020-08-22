# [LeetCode39.组合总和](https://leetcode-cn.com/problems/combination-sum/)
## 问题描述
给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

说明：

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

### 示例
```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```
## 题解
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] nums, int target) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(nums,target,list, new ArrayList<>(),0, 0);
    return list;
    }

    public void backtrack(int[] nums,int target,List<List<Integer>> list, List<Integer> tempList,int cur, int start){
        if(cur==target) list.add(new ArrayList<>(tempList));
        
        for(int i = start; i < nums.length; i++){
            if(cur+nums[i]>target) break;
            tempList.add(nums[i]);
            backtrack(nums, target,list, tempList, cur+nums[i], i); 
            tempList.remove(tempList.size() - 1);
        }
    }   
}
```
