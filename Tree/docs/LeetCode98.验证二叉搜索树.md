# [LeetCode98.验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
## 题目描述
给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

- 节点的左子树只包含小于当前节点的数。
- 节点的右子树只包含大于当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

### 示例
```
输入:
    2
   / \
  1   3
输出: true
```
```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```
## 题解
### 解法一
```java
class Solution {
  public boolean isValidBST(TreeNode root) {
    Deque<TreeNode> stack = new LinkedList();
    double inorder = - Double.MAX_VALUE;

    while (!stack.isEmpty() || root != null) {
      while (root != null) {
        stack.push(root);
        root = root.left;
      }
      root = stack.pop();
      // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
      if (root.val <= inorder) {
          return false;
      }
      inorder = root.val;
      root = root.right;
    }
    return true;
  }
}
```
#### 复杂度分析
- 时间复杂度 : $O(n)$，其中 `n` 为二叉树的节点个数。二叉树的每个节点最多被访问一次，因此时间复杂度为 $O(n)$。

- 空间复杂度 : $O(n)$，其中 `n` 为二叉树的节点个数。栈最多存储 `n` 个节点，因此需要额外的 $O(n)$ 的空间。

### 解法二
```java
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 访问左子树
        if (!isValidBST(root.left)) {
            return false;
        }
        // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;
        // 访问右子树
        return isValidBST(root.right);
    }
}
```
### 算法三
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        TreeNode maxLeft = root.left, minRight = root.right;
        // 找寻左子树中的最右（数值最大）节点
        while (maxLeft != null && maxLeft.right != null)
            maxLeft = maxLeft.right;
        // 找寻右子树中的最左（数值最小）节点
        while (minRight != null && minRight.left != null)
            minRight = minRight.left;
        // 当前层是否合法
        boolean ret = (maxLeft == null || maxLeft.val < root.val) && (minRight == null || root.val < minRight.val);
        // 进入左子树和右子树并判断是否合法
        return  ret && isValidBST(root.left) && isValidBST(root.right);
    }
}
```