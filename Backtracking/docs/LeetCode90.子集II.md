# [LeetCode90.子集II](https://leetcode-cn.com/problems/subsets-ii/)
## 题目描述
给定一个**可能包含重复元素**的整数数组 `nums`，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

### 示例
```
输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```
## 题解
### 解法一
```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    Arrays.sort(nums); //排序
    getAns(nums, 0, new ArrayList<>(), ans);
    return ans;
}

private void getAns(int[] nums, int start, ArrayList<Integer> temp, List<List<Integer>> ans) {
    ans.add(new ArrayList<>(temp));
    for (int i = start; i < nums.length; i++) {
        //和上个数字相等就跳过
        if (i > start && nums[i] == nums[i - 1]) {
            continue;
        }
        temp.add(nums[i]);
        getAns(nums, i + 1, temp, ans);
        temp.remove(temp.size() - 1);
    }
}
```
如何避免重复：
这个避免重复当思想是在是太重要了。
这个方法最重要的作用是，可以让同一层级，不出现相同的元素。即
```
                  1
                 / \
                2   2  这种情况不会发生 但是却允许了不同层级之间的重复即：
               /     \
              5       5
                例2
                  1
                 /
                2      这种情况确是允许的
               /
              2  
```
为何会有这种神奇的效果呢？

首先 `cur-1 == cur` 是用于判定当前元素是否和之前元素相同的语句。这个语句就能砍掉例1。
可是问题来了，如果把所有当前与之前一个元素相同的都砍掉，那么例二的情况也会消失。 
因为当第二个2出现的时候，他就和前一个2相同了。
                
那么如何保留例2呢？

那么就用`cur > begin` 来避免这种情况，你发现例1中的两个2是处在同一个层级上的，
例2的两个2是处在不同层级上的。
在一个`for`循环中，所有被遍历到的数都是属于一个层级的。我们要让一个层级中，
必须出现且只出现一个2，那么就放过第一个出现重复的2，但不放过后面出现的2。
第一个出现的2的特点就是 `cur == begin`. 第二个出现的2 特点是`cur > begin`.
### 解法二
```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    ans.add(new ArrayList<>());// 初始化空数组
    Arrays.sort(nums);
    int start = 1; //保存新解的开始位置
    for (int i = 0; i < nums.length; i++) {
        List<List<Integer>> ans_tmp = new ArrayList<>();
        // 遍历之前的所有结果
        for (int j = 0; j < ans.size(); j++) {
            List<Integer> list = ans.get(j);
            //如果出现重复数字，就跳过所有旧解
            if (i > 0 && nums[i] == nums[i - 1] && j < start) {
                continue;
            }
            List<Integer> tmp = new ArrayList<>(list);
            tmp.add(nums[i]); // 加入新增数字
            ans_tmp.add(tmp);
        }

        start = ans.size(); //更新新解的开始位置
        ans.addAll(ans_tmp);
    }
    return ans;
}
```
### 解法三
```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        int len=nums.length;
        boolean[] used=new boolean[len];
        Arrays.sort(nums);
        if(nums.length!=0){
            res.add(new LinkedList<>(path));
        }
        helper(res,path,nums,used,len);
        return res;
    }
    public void helper(List<List<Integer>> res,LinkedList<Integer> path,int[] nums,boolean[] used,int len){
        if(path.size()==len){
            return;
        }
        for(int i=0;i<len;++i){
            if(used[i]) continue;
            if(i>0&&nums[i]==nums[i-1]&&!used[i-1]) continue;
            if(path.size()==0||(path.size()!=0&&nums[i]>=path.getLast())){
                used[i]=true;
                path.addLast(nums[i]);
                res.add(new LinkedList<>(path));
                helper(res,path,nums,used,len);
                path.removeLast();
                used[i]=false;
            }
        }
    }
}
```
改进:
```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        int len=nums.length;
        boolean[] used=new boolean[len];
        Arrays.sort(nums);
        if(nums.length!=0){
            res.add(new LinkedList<>(path));
        }
        helper(0,res,path,nums,used,len);
        return res;
    }
    public void helper(int start,List<List<Integer>> res,LinkedList<Integer> path,int[] nums,boolean[] used,int len){
        if(path.size()==len){
            return;
        }
        for(int i=start;i<len;++i){
            if(used[i]) continue;
            if(i>0&&nums[i]==nums[i-1]&&!used[i-1]) continue;
            used[i]=true;
            path.addLast(nums[i]);
            res.add(new LinkedList<>(path));
            helper(i+1,res,path,nums,used,len);
            path.removeLast();
            used[i]=false;
        }
    }
}
```