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
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200822175730.png)
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200920185132.png)
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200822175952.png)
```
这个避免重复的思想是在是太重要了。

这个方法最重要的作用是，可以让同一层级，不出现相同的元素。即
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
                
为何会有这种神奇的效果呢？
首先 cur-1 == cur 是用于判定当前元素是否和之前元素相同的语句。这个语句就能砍掉例1。
可是问题来了，如果把所有当前与之前一个元素相同的都砍掉，那么例二的情况也会消失。 
因为当第二个2出现的时候，他就和前一个2相同了。
                
那么如何保留例2呢？
那么就用cur > begin 来避免这种情况，你发现例1中的两个2是处在同一个层级上的，
例2的两个2是处在不同层级上的。
在一个for循环中，所有被遍历到的数都是属于一个层级的。我们要让一个层级中，
必须出现且只出现一个2，那么就放过第一个出现重复的2，但不放过后面出现的2。
第一个出现的2的特点就是 cur == begin. 第二个出现的2 特点是cur > begin.
```
```java
public class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int len = candidates.length;
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) {
            return res;
        }
        // 先将数组排序，这一步很关键
        Arrays.sort(candidates);
        List<Integer> path = new ArrayList<>();
        backtrack(candidates,target,len,0,0, path, res);
        return res;
    }
    public void backtrack(int[] candidates,int target, int len, int begin, int sum, List<Integer> path, List<List<Integer>> res) {
        if (sum == target) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = begin; i < len; i++) {
            // 大剪枝
            if (sum+candidates[i]>target) {
                break;
            }
            // 小剪枝
            if (i > begin && candidates[i] == candidates[i - 1]) {
                continue;
            }
            path.add(candidates[i]);
            // 因为元素不可以重复使用，这里递归传递下去的是 i + 1 而不是 i
            backtrack(candidates,target,len, i + 1,sum+candidates[i], path, res);
            path.remove(path.size()-1);
        }
    }
}
```
如果没有去重步骤：
```java

class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res=new ArrayList<>();
        List<Integer> path=new ArrayList<>();
        Arrays.sort(candidates);
        boolean[] used=new boolean[candidates.length];
        helper(candidates,target,0,res,path,used);
        return res;
    }
    public void helper(int[] nums,int target,int sum,List<List<Integer>> res,List<Integer> path,boolean[] used){
        if(sum==target){
            res.add(new LinkedList<>(path));
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]) continue;
            if(i>0&&nums[i]==nums[i-1]&&!used[i-1]) continue;
            if(path.size()==0||(path.size()!=0)&&nums[i]>=path.get(path.size()-1)){
                if(sum+nums[i]<=target){
                    used[i]=true;
                    path.add(nums[i]);
                    helper(nums,target,sum+nums[i],res,path,used);
                    path.remove(path.size()-1);
                    used[i]=false;
                }
            }
        }
    }
}
```
