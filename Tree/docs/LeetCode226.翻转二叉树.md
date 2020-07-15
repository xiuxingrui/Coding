# [LeetCode226.翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
## 题目描述
翻转一棵二叉树。
### 示例
```
输入：

     4
   /   \
  2     7
 / \   / \
1   3 6   9

输出：

     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
## 题解
### 解法一:递归
#### 写法一：接收返回值

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }
    TreeNode right = invertTree(root.right);
    TreeNode left = invertTree(root.left);
    root.left = right;
    root.right = left;
    return root;
}
```
#### 写法二:不接收返回值
##### 前序遍历
```java
class Solution {
	public TreeNode invertTree(TreeNode root) {
		//递归函数的终止条件，节点为空时返回
		if(root==null) {
			return null;
		}
		//下面三句是将当前节点的左右子树交换
		TreeNode tmp = root.right;
		root.right = root.left;
		root.left = tmp;
		//递归交换当前节点的 左子树
		invertTree(root.left);
		//递归交换当前节点的 右子树
		invertTree(root.right);
		//函数返回时就表示当前这个节点，以及它的左右子树
		//都已经交换完了
		return root;
	}
}
```
##### 中序遍历
```java
public class Solution {

    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }

        invertTree(root.left);

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        // 注意：因为左右子树已经交换了，因此这里不能写 invertTree(root.right);
        // 即：这里的 root.left 就是交换之前的 root.right
        invertTree(root.left);
        return root;
    }
}
```
##### 后序遍历
```java
public class Solution {

    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }

        invertTree(root.left);
        invertTree(root.right);

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        return root;
    }
}
```
#### 复杂度分析
既然树中的每个节点都只被访问一次，那么时间复杂度就是 $O(n)$，其中 `n` 是树中节点的个数。在反转之前，不论怎样我们至少都得访问每个节点至少一次，因此这个问题无法做地比 $O(n)$ 更好了。

本方法使用了递归，在最坏情况下栈内需要存放 $O(h)$ 个方法调用，其中 `h` 是树的高度。由于$h \in O(n)$ ，可得出空间复杂度为 $O(n)$。

### 解法二：迭代
#### BFS
```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        TreeNode temp = current.left;
        current.left = current.right;
        current.right = temp;
        if (current.left != null) queue.add(current.left);
        if (current.right != null) queue.add(current.right);
    }
    return root;
}
```
#### DFS
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
    public TreeNode invertTree(TreeNode root) {
    Deque<TreeNode> stack = new LinkedList<>();
    stack.offerLast(root);
    while (!stack.isEmpty()) {
        TreeNode cur = stack.pollLast();
        if (cur == null) {
            continue;
        }
        TreeNode temp = cur.left;
        cur.left = cur.right;
        cur.right = temp;

        stack.offerLast(cur.right);
        stack.offerLast(cur.left);
    }
    return root;
    }
}
```
#### 复杂度分析
既然树中的每个节点都只被访问/入队一次，时间复杂度就是 $O(n)$，其中 `n` 是树中节点的个数。

空间复杂度是 $O(n)$，即使在最坏的情况下，也就是队列里包含了树中所有的节点。对于一颗完整二叉树来说，叶子节点那一层拥有$\left\lceil\frac{n}{2}\right\rceil=O(n)$个节点。



