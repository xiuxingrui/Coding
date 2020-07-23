# [LeetCode513.找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)
## 题目描述
给定一个二叉树，在树的最后一行找到最左边的值。
### 示例
```java
输入:

    2
   / \
  1   3

输出:
1
输入:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

输出:
7
```
## 题解
### 解法一
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
    int maxDepth = -1, res = -1;

    public int findBottomLeftValue(TreeNode root) {
        helper(root, 0);
        return res;
    }
    private void helper(TreeNode root, int depth) {
        if (root == null) return;
        helper(root.left, depth + 1);
        //判断是否是最大深度
        if (depth > maxDepth) {
            maxDepth = depth;
            res = root.val;
        }
        helper(root.right, depth + 1);
    }
}
```
### 解法二
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
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue=new LinkedList<>();
        queue.offer(root);
        int ans=0;
        while(!queue.isEmpty()){
            TreeNode temp=queue.poll();
            ans=temp.val;
            if(temp.right!=null){
                queue.offer(temp.right);
            }
            if(temp.left!=null){
                queue.offer(temp.left);
            }
        }
        return ans;
    }
}
```
