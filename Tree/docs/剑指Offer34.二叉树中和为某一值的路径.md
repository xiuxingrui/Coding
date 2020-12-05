# [剑指Offer34.二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
## 题目描述
输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

- `节点总数 <= 10000`
### 示例
给定如下二叉树，以及目标和 `sum = 22`，
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```
## 题解
## 题解
```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> result = new ArrayList<>();
        recur(result, new ArrayList<Integer>(), root, sum);
        return result;
    }

    private void recur(List<List<Integer>> result, List<Integer> curPath, TreeNode curNode, int sum){
        if(curNode == null){
            return;
        }
        curPath.add(curNode.val);
        if(curNode.val == sum && curNode.left == null && curNode.right == null){
            result.add(new ArrayList<>(curPath));
        }
        recur(result, curPath, curNode.left, sum - curNode.val);
        recur(result, curPath, curNode.right, sum - curNode.val);
        curPath.remove(curPath.size() - 1);
    }
}
```
或
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> ans=new ArrayList<>();
        backtracking(root,sum,new ArrayList<Integer>(),ans);
        return ans;
    }
    public void backtracking(TreeNode root,int sum,List<Integer> path,List<List<Integer>> ans){
        if(root==null){
            return;
        }
        if(root.left==null&&root.right==null){
            if(root.val==sum){
                path.add(root.val);
                ans.add(new ArrayList<>(path));
                path.remove(path.size()-1);
            }
            return;
        }
        path.add(root.val);
        backtracking(root.left,sum-root.val,path,ans);
        backtracking(root.right,sum-root.val,path,ans);
        path.remove(path.size()-1);
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N^2)$，其中 `N` 是树的节点数。在最坏情况下，树的上半部分为链状，下半部分为完全二叉树，并且从根节点到每一个叶子节点的路径都符合题目要求。此时，路径的数目为 $O(N)$，并且每一条路径的节点个数也为 $O(N)$，因此要将这些路径全部添加进答案中，时间复杂度为 $O(N^2)$。
- 空间复杂度：$O(N)$，其中 `N` 是树的节点数。空间复杂度主要取决于栈空间的开销，栈中的元素个数不会超过树的节点数。
