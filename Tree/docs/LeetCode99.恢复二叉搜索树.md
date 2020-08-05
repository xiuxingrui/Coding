# [LeetCode99.恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)
## 题目描述
二叉搜索树中的两个节点被错误地交换。

请在不改变其结构的情况下，恢复这棵树。

- 使用 $O(n)$ 空间复杂度的解法很容易实现。
- 你能想出一个只使用常数空间的解决方案吗？(要用到莫里斯遍历)
### 示例
```j
输入: [1,3,null,null,2]

   1
  /
 3
  \
   2

输出: [3,1,null,null,2]

   3
  /
 1
  \
   2
```
```
输入: [3,1,4,null,null,2]

  3
 / \
1   4
   /
  2

输出: [2,1,4,null,null,3]

  2
 / \
1   4
   /
  3
```
## 题解
### 解法一
中序遍历顺序是 `4,2,3,1`,我们只要找到节点4和节点1交换顺序即可!

这里我们有个规律发现这两个节点:

1. 第一个节点,是第一个按照中序遍历时候前一个节点大于后一个节点,我们选取前一个节点,这里指节点4;
2. 第二个节点,是在第一个节点找到之后, 后面出现前一个节点大于后一个节点,我们选择后一个节点,这里指节点1;

递归版本：
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
    TreeNode firstNode=null;
    TreeNode secondNode=null;
    TreeNode preNode=new TreeNode(Integer.MIN_VALUE);
    public void recoverTree(TreeNode root) {
        inorder(root);
        int temp=firstNode.val;
        firstNode.val=secondNode.val;
        secondNode.val=temp;
    }
    public void inorder(TreeNode root){
        if(root==null){
            return;
        }
        inorder(root.left);
        if(firstNode==null&&preNode.val>root.val){
            firstNode=preNode;
        }
        if(firstNode!=null&&preNode.val>root.val){
            secondNode=root;
        }
        preNode=root;
        inorder(root.right);
    }
}
```
迭代版本:

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
    public void recoverTree(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode firstNode = null;
        TreeNode secondNode = null;
        TreeNode pre = new TreeNode(Integer.MIN_VALUE);
        TreeNode p = root;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            if (firstNode == null && pre.val > p.val) firstNode = pre;
            if (firstNode != null && pre.val > p.val) secondNode = p;
            pre = p;
            p = p.right;
        }
        int tmp = firstNode.val;
        firstNode.val = secondNode.val;
        secondNode.val = tmp;
    }
}
```