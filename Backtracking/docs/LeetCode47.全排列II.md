# [LeetCode47.全排列II](https://leetcode-cn.com/problems/permutations-ii/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liwe-2/)
## 题目描述
给定一个可包含重复数字的序列，返回所有不重复的全排列。

### 示例
```
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```
## 题解
```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        int len=nums.length;
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        boolean[] used=new boolean[len];
        Arrays.sort(nums);
        helper(path,res,nums,used,len);
        return res;
    }
    public void helper(LinkedList<Integer> path,List<List<Integer>> res,int[] nums,boolean[] used,int len){
        if(path.size()==len){
            res.add(new LinkedList<>(path));
            return;
        }
        for(int i=0;i<len;++i){
            if(used[i]) continue;
            if(i>0&&nums[i]==nums[i-1]&&!used[i-1]){
                continue;
            }
            used[i]=true;
            path.addLast(nums[i]);
            helper(path,res,nums,used,len);
            path.removeLast();
            used[i]=false;
        }
    }
}
```
