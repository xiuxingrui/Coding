# [LeetCode216.组合总和III](https://leetcode-cn.com/problems/combination-sum-iii/)
## 题目描述
找出所有相加之和为 `n` 的 `k` 个数的组合。组合中只允许含有 `1 - 9` 的正整数，并且每种组合中不存在重复的数字。

说明：

- 所有数字都是正整数。
- 解集不能包含重复的组合。 

### 示例
```
输入: k = 3, n = 7
输出: [[1,2,4]]

输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
```
## 题解
### 解法一
```java
public class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        backtrack(k, n,0,0,1, path, res);
        return res;
    }
    public void backtrack(int k,int n,int sum,int cnt,int start, List<Integer> path, List<List<Integer>> res) {
        if (cnt==k) {
            if (sum == n) {
                res.add(new ArrayList<>(path));
            }
            return;
        }
        for (int i = start; i <= 9; i++) {
            if(sum+i>n){
                break;
            }
            path.add(i);
            backtrack(k,n,sum+i,cnt+1,i + 1, path, res);
            path.remove(path.size()-1);
        }
    }
}
```
### 解法二
```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res=new ArrayList<>();
        LinkedList<Integer> path=new LinkedList<>();
        boolean[] used=new boolean[9];
        helper(res,path,used,k,n);
        return res;
    }
    public void helper(List<List<Integer>> res,LinkedList<Integer> path,boolean[] used,int k,int n){
        if(path.size()==k){
            if(n==0){
                res.add(new LinkedList(path));
            }
            return;
        }
        for(int i=1;i<=9;++i){
            if(used[i-1]) continue;
            if(path.size()==0||path.size()!=0&&i>path.getLast()){
                used[i-1]=true;
                path.addLast(i);
                helper(res,path,used,k,n-i);
                path.removeLast();
                used[i-1]=false;
            }
        }
    }
}
```