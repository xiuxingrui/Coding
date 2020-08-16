# [LeetCode110.平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)
## 题目描述
给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

### 示例
```
给定二叉树 [3,9,20,null,null,15,7]
    3
   / \
  9  20
    /  \
   15   7
返回 true 。
```
```
给定二叉树 [1,2,2,3,3,null,null,4,4]
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 。
```
## 题解
### 解法一
由于自顶向下对于同一个节点，函数 `height` 会被重复调用，导致时间复杂度较高。如果使用自底向上的做法，则对于每个节点，函数 `height` 只会被调用一次。

当我们求左子树的高度时，同样是利用了递归去求它的左子树的高度和右子树的高度。当代码执行到
```java
isBalanced(root.left) && isBalanced(root.right)
```
递归的判断左子树和右子树是否是平衡二叉树的时候，我们又会继续求高度，求高度再次进入 `getTreeDepth` 函数的时候，我们会发现，其实在上一次这些高度都已经求过了。

第二个不好的地方在于， `getTreeDepth` 递归的求高度的时候，也是求了左子树的高度，右子树的高度，此时完全可以判断当前树是否是平衡二叉树了，而不是再继续求高度。

综上，我们其实只需要求一次高度，并且在求左子树和右子树的高度的同时，判断一下当前是否是平衡二叉树。

自底向上递归的做法类似于后序遍历，对于当前遍历到的节点，先递归地判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡。如果一棵子树是平衡的，则返回其高度（高度一定是非负整数），否则返回 −1。如果存在一棵子树不平衡，则整个二叉树一定不平衡。

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) >= 0;
    }

    public int height(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
        if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是二叉树中的节点个数。使用自底向上的递归，每个节点的计算高度和判断是否平衡都只需要处理一次，最坏情况下需要遍历二叉树中的所有节点，因此时间复杂度是 $O(n)$。
- 空间复杂度：$O(n)$，其中 `n` 是二叉树中的节点个数。空间复杂度主要取决于递归调用的层数，递归调用的层数不会超过 `n`。
### 解法二
定义函数 `height`，用于计算二叉树中的任意一个节点 `p` 的高度：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200817002758.png)

有了计算节点高度的函数，即可判断二叉树是否平衡。具体做法类似于二叉树的前序遍历，即对于当前遍历到的节点，首先计算左右子树的高度，如果左右子树的高度差是否不超过 1，再分别递归地遍历左右子节点，并判断左子树和右子树是否平衡。这是一个自顶向下的递归的过程。

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        } else {
            return isBalanced(root.left) && isBalanced(root.right)&&Math.abs(height(root.left) - height(root.right)) <= 1;
        }
    }

    public int height(TreeNode root) {
        if (root == null) {
            return 0;
        } else {
            return Math.max(height(root.left), height(root.right)) + 1;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n^2)$，其中 `n` 是二叉树中的节点个数。最坏情况下，二叉树是满二叉树，需要遍历二叉树中的所有节点，时间复杂度是 $O(n)$。对于节点 `p`，如果它的高度是 `d`，则 `height(p)` 最多会被调用 `d` 次（即遍历到它的每一个祖先节点时）。对于平均的情况，一棵树的高度 `h` 满足 $O(h)=O(logn)$，因为 `d≤h`，所以总时间复杂度为 $O(nlogn)$。对于最坏的情况，二叉树形成链式结构，高度为 $O(n)$，此时总时间复杂度为 $O(n^2)$。
- 空间复杂度：$O(n)$，其中 `n` 是二叉树中的节点个数。空间复杂度主要取决于递归调用的层数，递归调用的层数不会超过 `n`。


