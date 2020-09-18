# [LeetCode47.全排列II](https://leetcode-cn.com/problems/permutations-ii/)
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
### 解法一
思路：在一定会产生重复结果集的地方剪枝。

如果要比较两个列表是否一样，一个很显然的办法是分别排序，然后逐个比对。既然要排序，我们可以在搜索之前就对候选数组排序，一旦发现这一支搜索下去可能搜索到重复的元素就停止搜索，这样结果集中不会包含重复元素。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200822162033.png)

产生重复结点的地方，正是图中标注了“剪刀”，且被绿色框框住的地方。

大家也可以把第 2 个 1 加上 `'` ，即 `[1, 1', 2]` 去想象这个搜索的过程。只要遇到起点一样，就有可能产生重复。这里还有一个很细节的地方：

1、在图中 ② 处，搜索的数也和上一次一样，但是上一次的 1 还在使用中；
2、在图中 ① 处，搜索的数也和上一次一样，但是上一次的 1 刚刚被撤销，正是因为刚被撤销，下面的搜索中还会使用到，因此会产生重复，剪掉的就应该是这样的分支。

```java
if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
    continue;
}
```
这段代码就能检测到标注为 ① 的两个结点，跳过它们。
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
### 解法二
`HashSet`去重：
```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        int len=nums.length;
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        boolean[] used=new boolean[len];
        helper(path,res,nums,used,len);
        return res;
    }
    public void helper(LinkedList<Integer> path,List<List<Integer>> res,int[] nums,boolean[] used,int len){
        if(path.size()==len){
            res.add(new LinkedList<>(path));
            return;
        }
        HashSet<Integer> set=new HashSet<>();
        for(int i=0;i<len;++i){
            if(used[i]) continue;
            if(set.contains(nums[i])){
                continue;
            }
            set.add(nums[i]);
            used[i]=true;
            path.addLast(nums[i]);
            helper(path,res,nums,used,len);
            path.removeLast();
            used[i]=false;
        }
    }
}
```
