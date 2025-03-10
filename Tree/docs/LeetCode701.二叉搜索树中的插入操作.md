# [LeetCode701.二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
## 题目描述
给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 保证原始二叉搜索树中不存在新值。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回任意有效的结果。

```
给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和 插入的值: 5
```
你可以返回这个二叉搜索树:
```
       4
       /   \
      2     7
     / \   /
    1   3 5
```
或者这个树也是有效的:
```
        5
       /   \
      2     7
     / \   
    1   3
         \
          4
```
## 题解
### 解法一:递归
```java
class Solution {
  public TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null){
      return new TreeNode(val);
    }

    // insert into the right subtree
    if (val > root.val) {
      root.right = insertIntoBST(root.right, val);
    }else {
      root.left = insertIntoBST(root.left, val);// insert into the left subtree
    }
    return root;
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(H)$，其中 `H` 指的是树的高度。平均情况下$O(logN)$，最坏的情况下$O(N)$。
- 空间复杂度：平均情况下$O(H)$。最坏的情况下是$O(N)$，是在递归过程中堆栈使用的空间。
### 解法二：迭代
```java
class Solution {
  public TreeNode insertIntoBST(TreeNode root, int val) {
    TreeNode node = root;
    while (node != null) {
      // insert into the right subtree
      if (val > node.val) {
        // insert right now
        if (node.right == null) {
          node.right = new TreeNode(val);
          return root;
        }
        else node = node.right;
      }
      // insert into the left subtree
      else {
        // insert right now
        if (node.left == null) {
          node.left = new TreeNode(val);
          return root;
        }
        else node = node.left;
      }
    }
    return new TreeNode(val);
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(H)$，其中 `H` 指的是树的高度。平均情况下$O(logN)$，最坏的情况下$O(N)$。
- 空间复杂度：$O(1)$
