# [LeetCode285.二叉搜索树中的顺序后继](https://leetcode-cn.com/problems/inorder-successor-in-bst/)
## 题目描述
给你一个二叉搜索树和其中的某一个结点，请你找出该结点在树中顺序后继的节点。

结点 `p` 的后继是值比 `p.val` 大的结点中键值最小的结点。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200720192337.png)

```
输入: root = [2,1,3], p = 1
输出: 2
解析: 这里 1 的顺序后继是 2。请注意 p 和返回值都应是 TreeNode 类型。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200720192349.png)

```
输入: root = [5,3,6,2,4,null,null,1], p = 6
输出: null
解析: 因为给出的结点没有顺序后继，所以答案就返回 null 了。
```
## 题解
### 解法一：栈
```java
class Solution {
  public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    // the successor is somewhere lower in the right subtree
    // successor: one step right and then left till you can
    if (p.right != null) {
      p = p.right;
      while (p.left != null) p = p.left;
      return p;
    }

    // the successor is somewhere upper in the tree
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();
    int inorder = Integer.MIN_VALUE;

    // inorder traversal : left -> node -> right
    while (!stack.isEmpty() || root != null) {
      // 1. go left till you can
      while (root != null) {
        stack.push(root);
        root = root.left;
      }

      // 2. all logic around the node
      root = stack.pop();
      // if the previous node was equal to p
      // then the current node is its successor
      if (inorder == p.val) return root;
      inorder = root.val;

      // 3. go one step right
      root = root.right;
    }

    // there is no successor
    return null;
  }
}
```
### 解法二:递归
写法一：
```java
public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    if(p==null||root==null){
        return null;
    }
    if(root.val<=p.val){//当前和左边都不可能>p
        return inorderSuccessor(root.right,p);
    }
    //root>p
    TreeNode res1=inorderSuccessor(root.left,p);
    if(res1!=null&&res1.val<root.val){
        return res1;
    }else{
        return root;
    }
}
```
写法二：
```java
//中序遍历，找到P节点后遍历的下一个节点就是顺序后继节点
TreeNode res;
boolean findP = false;

public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    if (root == null) return null;
    helper(root, p);
    //判断p是否是最后一个节点
    if (findP) res = null;
    return res;
}

private void helper(TreeNode root, TreeNode p) {
    if (root == null) return;
    helper(root.left, p);
    //如果已经找到p，则遍历的下一个节点为res
    if (findP) {
        res = root;
        findP = false;
        return;
    }
    //判断当前节点是否是p
    if (root == p) {
        findP = true;
    }
    helper(root.right, p);
}
```
### 解法三
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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode ans=null;
        while(root!=null){
            if(root.val>p.val){
                ans=root;
                root=root.left;
            }else{
                root=root.right;
            }
        }
        return ans;
    }
}
```
