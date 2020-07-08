# [剑指Offer28.对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)
## 题目描述
给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:
```
   1
   / \
  2   2
   \   \
   3    3
```
## 题解
### 解法一:
递归
```java
class Solution {
	public boolean isSymmetric(TreeNode root) {
		if(root==null) {
			return true;
		}
		//调用递归函数，比较左节点，右节点
		return dfs(root.left,root.right);
	}
	
	boolean dfs(TreeNode left, TreeNode right) {
		//递归的终止条件是两个节点都为空
		//或者两个节点中有一个为空
		//或者两个节点的值不相等
		if(left==null && right==null) {
			return true;
		}
		if(left==null || right==null) {
			return false;
		}
		if(left.val!=right.val) {
			return false;
		}
		//再递归的比较 左节点的左孩子 和 右节点的右孩子
		//以及比较  左节点的右孩子 和 右节点的左孩子
		return dfs(left.left,right.right) && dfs(left.right,right.left);
	}
}
```
#### 复杂度分析

假设树上一共 `n` 个节点。

- 时间复杂度：这里遍历了这棵树，渐进时间复杂度为 $O(n)$。
- 空间复杂度：这里的空间复杂度和递归使用的栈空间有关，这里递归层数不超过 `n`，故渐进空间复杂度为 $O(n)$。
### 解法二
迭代
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root.left);
        queue.offer(root.right);

        while (!queue.isEmpty()) {
            // 每次出队两个节点 node1 和 node2
            TreeNode node1 = queue.poll();
            TreeNode node2 = queue.poll();
            // 首先比较 node1 与 node2 这两个节点的值是否相等
            if (node1 == null && node2 == null) {
                continue;
            }
            if (node1 == null || node2 == null || node1.val != node2.val) {
                return false;
            }
            // 再将 node1 的左节点与 node2 的右节点一起入队（以便两节点一起出队，进行比较）
            queue.offer(node1.left);
            queue.offer(node2.right);
            // 再将 node1 的右节点与 node2 的左节点一起入队（以便两节点一起出队，进行比较）
            queue.offer(node1.right);
            queue.offer(node2.left);
        }

        return true;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(n)$。
- 空间复杂度：这里需要用一个队列来维护节点，每个节点最多进队一次，出队一次，队列中最多不会超过 `n` 个点，故渐进空间复杂度为 $O(n)$。
