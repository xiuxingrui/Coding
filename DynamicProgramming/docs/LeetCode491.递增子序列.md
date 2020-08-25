# [LeetCode491.递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/)
## 题目描述
给定一个整型数组, 你的任务是找到所有该数组的递增子序列，递增子序列的长度至少是2。

- 给定数组的长度不会超过15。
- 数组中的整数范围是 `[-100,100]`。
- 给定数组中可能包含重复数字，相等的数字应该被视为递增的一种情况。
### 示例
```
输入: [4, 6, 7, 7]
输出: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
```
## 题解
### 解法一
```java
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> temp=new ArrayList<>();
        backtrack(nums,0,ans,temp);
        return ans;
    }
    public void backtrack(int[] nums,int start,List<List<Integer>> ans,List<Integer> temp){
        if(temp.size()>=2){
                ans.add(new ArrayList(temp));
        }
        // 借助 set 对 [start, nums.length - 1] 范围内的数去重。
        HashSet<Integer> set = new HashSet<>();
        for(int i=start;i<nums.length;i++){
            if(set.contains(nums[i])){
                continue;
            }
            //set.add(nums[i]);写在这里也行，这里是对所有取值去重
            int size=temp.size();
            if(size==0||size>0&&nums[i]>=temp.get(size-1)){
                set.add(nums[i]);//由于考虑的是有效值，这里是对有效取值去重
                temp.add(nums[i]);
                backtrack(nums,i+1,ans,temp);
                temp.remove(temp.size()-1);
            }
        }
    }
}
```
### 解法二
暴力去重
```java
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> temp=new ArrayList<>();
        HashSet<List<Integer>> set=new HashSet<>();
        backtrack(nums,0,ans,temp,set);
        return ans;
    }
    public void backtrack(int[] nums,int start,List<List<Integer>> ans,List<Integer> temp,HashSet<List<Integer>> set){
        if(temp.size()>=2){
            if(set.contains(temp)==false){
                ArrayList<Integer> ansTemp=new ArrayList(temp);
                ans.add(ansTemp);
                set.add(ansTemp);
            }
        }
        for(int i=start;i<nums.length;i++){
            int size=temp.size();
            if(size==0||size>0&&nums[i]>=temp.get(size-1)){
                temp.add(nums[i]);
                backtrack(nums,i+1,ans,temp,set);
                temp.remove(temp.size()-1);
            }
        }
    }
}
```