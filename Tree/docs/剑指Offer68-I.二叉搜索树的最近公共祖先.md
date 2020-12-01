# [剑指Offer68-I.二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)
## 题目描述
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 `T` 的两个结点 `p`、`q`，最近公共祖先表示为一个结点 `x`，满足 `x` 是 `p`、`q` 的祖先且 `x` 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  `root = [6,2,8,0,4,7,9,null,null,3,5]`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201201141635.png)

- 所有节点的值都是唯一的。
- `p`、`q` 为不同节点且均存在于给定的二叉搜索树中。
### 示例
```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```
```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```
## 题解
### 解法一
版本1：
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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null||root==p||root==q){
            return root;
        }
        TreeNode leftNode,rightNode;
        if(root.val<p.val&&root.val<q.val){
            leftNode=null;
        }else{
            leftNode=lowestCommonAncestor(root.left,p,q);
        }
        if(root.val>p.val&&root.val>q.val){
            rightNode=null;
        }else{
            rightNode=lowestCommonAncestor(root.right,p,q);
        }
        if(leftNode==null){
            return rightNode;
        }
        if(rightNode==null){
            return leftNode;
        }
        return root;
    }
}
```
版本2：

1. 从根节点开始遍历树
2. 如果节点`p` 和节点 `q` 都在右子树上，那么以右孩子为根节点继续 1 的操作
3. 如果节点 `p` 和节点 `q` 都在左子树上，那么以左孩子为根节点继续 1 的操作
4. 如果条件 2 和条件 3 都不成立，这就意味着我们已经找到节点 `p` 和节点 `q` 的 `LCA` 了

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        // Value of current node or parent node.
        int parentVal = root.val;

        // Value of p
        int pVal = p.val;

        // Value of q;
        int qVal = q.val;

        if (pVal > parentVal && qVal > parentVal) {
            // If both p and q are greater than parent
            return lowestCommonAncestor(root.right, p, q);
        } else if (pVal < parentVal && qVal < parentVal) {
            // If both p and q are lesser than parent
            return lowestCommonAncestor(root.left, p, q);
        } else {
            // We have found the split point, i.e. the LCA node.
            return root;
        }
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(N)$:其中 `N` 为 `BST` 中节点的个数，在最坏的情况下我们可能需要访问 `BST` 中所有的节点。

- 空间复杂度：$O(N)$
所需开辟的额外空间主要是递归栈产生的，之所以是 `N` 是因为 `BST` 的高度为 `N`。

### 解法二
我们用迭代的方式替代了递归来遍历整棵树。由于我们不需要回溯来找到 `LCA` 节点，所以我们是完全可以不利用栈或者是递归的。实际上这个问题本身就是可以迭代的，我们只需要找到分割点就可以了。这个分割点就是能让节点 `p` 和节点 `q` 不能在同一颗子树上的那个节点，或者是节点 `p` 和节点 `q` 中的一个，这种情况下其中一个节点是另一个节点的父亲节点。
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        // Value of p
        int pVal = p.val;

        // Value of q;
        int qVal = q.val;

        // Start from the root node of the tree
        TreeNode node = root;

        // Traverse the tree
        while (node != null) {

            // Value of ancestor/parent node.
            int parentVal = node.val;

            if (pVal > parentVal && qVal > parentVal) {
                // If both p and q are greater than parent
                node = node.right;
            } else if (pVal < parentVal && qVal < parentVal) {
                // If both p and q are lesser than parent
                node = node.left;
            } else {
                // We have found the split point, i.e. the LCA node.
                return node;
            }
        }
        return null;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$:其中 `N` 为 `BST` 中节点的个数，在最坏的情况下我们可能需要遍历 `BST` 中所有的节点。
- 空间复杂度：$O(1)$