# [LeetCode111.二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
## 题目描述
给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
### 示例
给定二叉树 `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最小深度  2.
## 题解
```
    1
   / 
  2 
```
应返回2而不是1.
### 解法一
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
    public int minDepth(TreeNode root) {
        if(root==null){
            return 0;
        }
        if(root.left==null){
            return minDepth(root.right)+1;
        }
        if(root.right==null){
            return minDepth(root.left)+1;
        }
        return Math.min(minDepth(root.left),minDepth(root.right))+1;
    }
}
```
写法二:
```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        if (root.left == null && root.right == null) {
            return 1;
        }

        int min_depth = Integer.MAX_VALUE;
        if (root.left != null) {
            min_depth = Math.min(minDepth(root.left), min_depth);
        }
        if (root.right != null) {
            min_depth = Math.min(minDepth(root.right), min_depth);
        }

        return min_depth + 1;
    }
}
```
#### 复杂度分析
- 时间复杂度：我们访问每个节点一次，时间复杂度为 $O(N)$，其中 `N` 是节点个数。
- 空间复杂度：最坏情况下，整棵树是非平衡的，例如每个节点都只有一个孩子，递归会调用 `N` （树的高度）次，因此栈的空间开销是 $O(N)$ 。但在最好情况下，树是完全平衡的，高度只有 $log(N)$，因此在这种情况下空间复杂度只有 $O(log(N))$。
### 解法二:栈
```java
class Solution {
  public int minDepth(TreeNode root) {
    LinkedList<Pair<TreeNode, Integer>> stack = new LinkedList<>();
    if (root == null) {
      return 0;
    }
    else {
      stack.add(new Pair(root, 1));
    }

    int min_depth = Integer.MAX_VALUE;
    while (!stack.isEmpty()) {
      Pair<TreeNode, Integer> current = stack.pollLast();
      root = current.getKey();
      int current_depth = current.getValue();
      if ((root.left == null) && (root.right == null)) {
        min_depth = Math.min(min_depth, current_depth);
      }
      if (root.left != null) {
        stack.add(new Pair(root.left, current_depth + 1));
      }
      if (root.right != null) {
        stack.add(new Pair(root.right, current_depth + 1));
      }
    }
    return min_depth;
  }
}
```
#### 复杂度分析
- 时间复杂度：每个节点恰好被访问一遍，复杂度为 $O(N)$。
- 空间复杂度：最坏情况下我们会在栈中保存整棵树，此时空间复杂度为 $O(N)$。
### 解法三:队列
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
    public int minDepth(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        if (root == null)
            return 0;
        queue.offer(root);
        int level = 0;
        while (!queue.isEmpty()) {
            level++;
            int levelNum = queue.size(); // 当前层元素的个数
            for (int i = 0; i < levelNum; i++) {
                TreeNode curNode = queue.poll();
                if (curNode != null) {
                    if (curNode.left == null && curNode.right == null) {
                        return level;
                    }
                    if (curNode.left != null) {
                        queue.offer(curNode.left);
                    }
                    if (curNode.right != null) {
                        queue.offer(curNode.right);
                    }
                }
            }
        }
        return level;
    }
}
```
#### 复杂度分析
- 时间复杂度：最坏情况下，这是一棵平衡树，我们需要按照树的层次一层一层的访问完所有节点，除去最后一层的节点。这样访问了 $N/2$ 个节点，因此复杂度是 $O(N)$。
- 空间复杂度：和时间复杂度相同，也是 $O(N)$。




