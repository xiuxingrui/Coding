# [LeetCode1373.二叉搜索子树的最大键值和](https://leetcode-cn.com/problems/maximum-sum-bst-in-binary-tree/)
## 题目描述
给你一棵以 `root` 为根的 二叉树 ，请你返回 任意 二叉搜索子树的最大键值和。

二叉搜索树的定义如下：

- 任意节点的左子树中的键值都 小于 此节点的键值。
- 任意节点的右子树中的键值都 大于 此节点的键值。
- 任意节点的左子树和右子树都是二叉搜索树。

提示：

- 每棵树最多有 40000 个节点。
- 每个节点的键值在 `[-4 * 10^4 , 4 * 10^4]`之间。
### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200805143046.png)
```
输入：root = [1,4,3,2,4,2,5,null,null,null,null,null,null,4,6]
输出：20
解释：键值为 3 的子树是和最大的二叉搜索树。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200805143122.png)
```
输入：root = [4,3,null,1,2]
输出：2
解释：键值为 2 的单节点子树是和最大的二叉搜索树。
```
```
输入：root = [-4,-2,-5]
输出：0
解释：所有节点键值都为负数，和最大的二叉搜索树为空。
```
```
输入：root = [2,1,3]
输出：6
```
```
输入：root = [5,4,8,3,null,6,3]
输出：7
```
## 题解
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int max=0;
    public int maxSumBST(TreeNode root) {
        preorder(root);
        return max;
    }
    public boolean isBST(TreeNode root){
        if (root == null) return true;
        if(!isBST(root.left) || !isBST(root.right)){
            return false;
        }
        TreeNode maxLeft = root.left, minRight = root.right;
        // 找寻左子树中的最右（数值最大）节点
        while (maxLeft != null && maxLeft.right != null)
            maxLeft = maxLeft.right;
        // 找寻右子树中的最左（数值最小）节点
        while (minRight != null && minRight.left != null)
            minRight = minRight.left;
        return (maxLeft == null || maxLeft.val < root.val) && (minRight == null || root.val < minRight.val);
    }
    public int sum(TreeNode root){
        if(root==null){
            return 0;
        }
        int sumAll=0;
        int left=sum(root.left);
        int right=sum(root.right);
        sumAll=root.val+left+right;
        max= Math.max(max, sumAll);
        return sumAll;
    }
    public void preorder(TreeNode root){
        if(root==null){
            return;
        }
        if(isBST(root)){
            sum(root);
            return;
        }
        preorder(root.left);
        preorder(root.right);
    }
}
```