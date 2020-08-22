# [LeetCode78.子集](https://leetcode-cn.com/problems/subsets/)
## 题目描述
给定一组**不含重复元素**的整数数组 `nums`，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

### 示例
```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
## 题解
### 解法一:
```java
public class Solution {

    private List<List<Integer>> res;

    private void find(int[] nums, int begin, List<Integer> pre) {
        // 没有显式的递归终止
        res.add(new ArrayList<>(pre));// 注意：Java 的引用传递机制，这里要 new 一下
        for (int i = begin; i < nums.length; i++) {
            pre.add(nums[i]);
            find(nums, i + 1, pre);
            pre.remove(pre.size() - 1);// 组合问题，状态在递归完成后要重置
        }
    }

    public List<List<Integer>> subsets(int[] nums) {
        int len = nums.length;
        res = new ArrayList<>();
        if (len == 0) {
            return res;
        }
        List<Integer> pre = new ArrayList<>();
        find(nums, 0, pre);
        return res;
    }
}
```
### 解法二
先只考虑给定数组的 1 个元素的所有子数组，然后再考虑数组的 2 个元素的所有子数组 ... 最后再考虑数组的 `n` 个元素的所有子数组。求 `k` 个元素的所有子数组，只需要在 `k - 1` 个元素的所有子数组里边加上 `nums [ k ]` 即可。
```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    ans.add(new ArrayList<>());//初始化空数组
    for(int i = 0;i<nums.length;i++){
        List<List<Integer>> ans_tmp = new ArrayList<>();
        //遍历之前的所有结果
        for(List<Integer> list : ans){
            List<Integer> tmp = new ArrayList<>(list);
            tmp.add(nums[i]); //加入新增数字
            ans_tmp.add(tmp);
        }
        ans.addAll(ans_tmp);
    }
    return ans;
}
```
### 解法三
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        boolean[] used=new boolean[nums.length];
        Arrays.sort(nums);
        int len=nums.length;
        if(nums.length!=0){
            res.add(new LinkedList(path));
        }
        helper(res,path,nums,used,len);
        
        return res;
    }
    public void helper(List<List<Integer>> res,LinkedList<Integer> path,int[] nums,boolean[] used,int len){
        
        if(path.size()==len){
            return;
        }
        for(int j=0;j<nums.length;++j){
            if(used[j]) continue;
            if(path.size()==0||(path.size()!=0&&nums[j]>path.getLast())){
                used[j]=true;
                path.addLast(nums[j]);
                res.add(new LinkedList<>(path));
                helper(res,path,nums,used,len);
                path.removeLast();
                used[j]=false;
            }
        }
    }
}
```