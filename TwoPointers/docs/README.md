# [LeetCode18.四数之和](https://leetcode-cn.com/problems/4sum/)
## 题目描述
给定一个包含 `n` 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 `a`，`b`，`c` 和 `d` ，使得 `a + b + c + d` 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

答案中不可以包含重复的四元组。

### 示例
```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```
## 题解
### 解法一

### 解法二
回溯+剪枝
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ans=new ArrayList<>();
        backtrack(nums,target,0,0,ans,new ArrayList<Integer>());
        return ans;
    }
    public void backtrack(int[] nums,int target,int start,int cnt,List<List<Integer>> ans,List<Integer> path){
        if(cnt==4){
            if(target==0){
                ans.add(new ArrayList<>(path));
            }
            return;
        }
        for(int i=start;i<nums.length;i++){
            if(i>start&&nums[i]==nums[i-1]){
                continue;
            }
            if(target<0&&nums[i]>=0){
                return;
            }
            path.add(nums[i]);
            backtrack(nums,target-nums[i],i+1,cnt+1,ans,path);
            path.remove(path.size()-1);
        }
    }
}
```