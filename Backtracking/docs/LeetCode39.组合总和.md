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
去重复:
- 在搜索的时候，需要设置搜索起点的下标 `start` ，由于一个数可以使用多次，下一层的结点从这个搜索起点开始搜索；
- 在搜索起点 `start` 之前的数因为以前的分支搜索过了，所以一定会产生重复。

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] nums, int target) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(nums,target,list, new ArrayList<>(),0, 0);
    return list;
    }

    public void backtrack(int[] nums,int target,List<List<Integer>> list, List<Integer> tempList,int sum, int start){
        if(sum==target) list.add(new ArrayList<>(tempList));
        
        for(int i = start; i < nums.length; i++){
            if(sum+nums[i]>target) break;
            tempList.add(nums[i]);
            backtrack(nums, target,list, tempList, sum+nums[i], i); 
            tempList.remove(tempList.size() - 1);
        }
    }   
}
```
如果没有上述的去重步骤，则
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res=new ArrayList<>();
        List<Integer> path=new ArrayList<>();
        Arrays.sort(candidates);
        helper(candidates,target,0,res,path);
        return res;
    }
    public void helper(int[] nums,int target,int sum,List<List<Integer>> res,List<Integer> path){
        if(sum==target){
            res.add(new LinkedList<>(path));
        }
        for(int i=0;i<nums.length;i++){
            if(path.size()==0||(path.size()!=0)&&nums[i]>=path.get(path.size()-1)){
                if(sum+nums[i]<=target){
                    path.add(nums[i]);
                    helper(nums,target,sum+nums[i],res,path);
                    path.remove(path.size()-1);
                }
            }
        }
    }
}
```
