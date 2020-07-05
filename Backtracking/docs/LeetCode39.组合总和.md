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
### 解法一
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] nums, int target) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, target, 0);
    return list;
    }

    public void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start){
        if(remain == 0) list.add(new ArrayList<>(tempList));
        
        for(int i = start; i < nums.length; i++){
            if(remain-nums[i]<0) break;
            tempList.add(nums[i]);
            backtrack(list, tempList, nums, remain - nums[i], i); 
            //找到了一个解或者 remain < 0 了，将当前数字移除，然后继续尝试
            tempList.remove(tempList.size() - 1);
        }
    }   
}
```
### 解法二
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        int len=candidates.length;
        Arrays.sort(candidates);
        helper(res,path,target,candidates,len);
        return res;
    }
    public void helper(List<List<Integer>> res,LinkedList<Integer> path,int target,int[] nums,int len){
        if(target<0) return;
        if(target==0){
            res.add(new LinkedList<>(path));
        }
        for(int i=0;i<len;++i){
            if(path.size()==0||(path.size()!=0)&&nums[i]>=path.getLast()){
                path.addLast(nums[i]);
                helper(res,path,target-nums[i],nums,len);
                path.removeLast();
            }
        }
    }
}
```
