# [LeetCode94.二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
## 题目描述
给定一个二叉树，返回它的中序 遍历。
### 示例
```
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```
## 题解
### 解法一:递归
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        inOrder(root, res);
        return res;
    }

    private void inOrder(TreeNode root, List<Integer> res) {
        if (root == null) return;
        inOrder(root.left, res);
        res.add(root.val);
        inOrder(root.right, res);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$。递归函数 $T(n)=2⋅T(n/2)+1$。
- 空间复杂度：最坏情况下需要空间$O(n)$，平均情况为$O(logn)$。
### 解法二:常规迭代
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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans=new ArrayList<>();
        Deque<TreeNode> stack=new LinkedList<>();
        TreeNode cur=root;
        while(!stack.isEmpty()||cur!=null){
            while(cur!=null){
                stack.offerFirst(cur);
                cur=cur.left;
            }
            cur=stack.pullFirst();
            ans.add(cur.val);
            cur=cur.right;
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$。
- 空间复杂度：$O(n)$。