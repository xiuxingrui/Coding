# [LeetCode700.二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)
## 题目描述
给定二叉搜索树（`BST`）的根节点和一个值。 你需要在BST中找到节点值等于给定值的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 `NULL`。

例如，

给定二叉搜索树:
```
        4
       / \
      2   7
     / \
    1   3
```
和值: 2
你应该返回如下子树:
```
      2     
     / \   
    1   3
```
在上述示例中，如果要找的值是 5，但因为没有节点值为 5，我们应该返回 `NULL`。
## 题解
### 解法一
递归实现非常简单：

- 如果根节点为空 `root == null` 或者根节点的值等于搜索值 `val == root.val`，返回根节点。
- 如果 `val < root.val`，进入根节点的左子树查找 `searchBST(root.left, val)`。
- 如果 `val > root.val`，进入根节点的右子树查找 `searchBST(root.right, val)`。

返回根节点。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200930003523.png)

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
    public TreeNode searchBST(TreeNode root, int val) {
        if(root==null){
            return null;
        }
        if(root.val==val){
            return root;
        }else if(root.val<val){
            return searchBST(root.right,val);
        }else{
            return searchBST(root.left,val);
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(H)$，其中 `H` 是树高。平均时间复杂度为$O(logN)$，最坏时间复杂度为$O(N)$。
- 空间复杂度：$O(H)$，递归栈的深度为 `H`。平均情况下深度为 $O(logN)$，最坏情况下深度为 $O(N)$。
### 解法二
为了降低空间复杂度，将递归转换为迭代：

- 如果根节点不空 `root != null` 且根节点不是目的节点 `val != root.val`：

- 如果 `val < root.val`，进入根节点的左子树查找 `root = root.left`。
- 如果 `val > root.val`，进入根节点的右子树查找 `root = root.right`。
- 返回 `root`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200930003903.png)

```java
class Solution {
  public TreeNode searchBST(TreeNode root, int val) {
    while (root != null && val != root.val)
      root = val < root.val ? root.left : root.right;
    return root;
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(H)$，其中 `H` 是树高。平均时间复杂度为$O(logN)$，最坏时间复杂度为$O(N)$。
- 空间复杂度：$O(1)$，恒定的额外空间。