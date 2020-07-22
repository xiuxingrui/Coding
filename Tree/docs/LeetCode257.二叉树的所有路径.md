# [LeetCode257.二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)
## 题目描述
给定一个二叉树，返回所有从根节点到叶子节点的路径。
### 示例
```
输入:

   1
 /   \
2     3
 \
  5

输出: ["1->2->5", "1->3"]

解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
```
## 题解
### 解法一:递归
写法一：
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
    public List<String> binaryTreePaths(TreeNode root) {
        LinkedList<String> paths = new LinkedList();
        construct_paths(root, "", paths);
        return paths;
    }
    public void construct_paths(TreeNode root, String path, LinkedList<String> paths) {
        if (root == null) {
            return;
        }
        path += Integer.toString(root.val);
        if ((root.left == null) && (root.right == null)){  // 当前节点是叶子节点
            paths.add(path);  // 把路径加入到答案中
        }
        path += "->";  // 当前节点不是叶子节点，继续递归遍历
        construct_paths(root.left, path, paths);
        construct_paths(root.right, path, paths);
    }
}
```
写法2：
```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private List<Integer> path = new ArrayList<>();

    public List<String> binaryTreePaths(TreeNode root) {
        dfs(root);
        return ans;
    }

    private void dfs(TreeNode root) {
        if(root == null) {
            return;
        }
        path.add(root.val);
        if(root.left == null && root.right == null) {
            StringBuilder temp = new StringBuilder();
            for(int i = 0; i < path.size(); i++) {
                temp.append(path.get(i));
                if(i != path.size() - 1) {
                    temp.append("->");
                }
            }
            ans.add(temp.toString());
        }
        dfs(root.left);
        dfs(root.right);
        path.remove(path.size() - 1);
    }
}
```
#### 复杂度分析
- 时间复杂度：每个节点只会被访问一次，因此时间复杂度为 $O(N)$，其中 `N` 表示节点数目。
- 空间复杂度：$O(N)$。这里不考虑存储答案 `paths` 使用的空间，仅考虑额外的空间复杂度。额外的空间复杂度为递归时使用的栈空间，在最坏情况下，当二叉树中每个节点只有一个孩子节点时，递归的层数为 `N`，此时空间复杂度为 $O(N)$。在最好情况下，当二叉树为平衡二叉树时，它的高度为 $log(N)$，此时空间复杂度为 $O(logN)$。
### 解法二:迭代
我们维护一个栈，存储节点以及根到该节点的路径。一开始这个栈里只有根节点。在每一步迭代中，我们取出栈顶，如果它是一个叶子节点，则将它对应的路径加入到答案中。如果它不是一个叶子节点，则将它的所有孩子节点入栈。当栈为空时，迭代结束。
```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        LinkedList<String> paths = new LinkedList();
        if (root == null)
            return paths;

        LinkedList<TreeNode> node_stack = new LinkedList();
        LinkedList<String> path_stack = new LinkedList();
        node_stack.add(root);
        path_stack.add(Integer.toString(root.val));
        TreeNode node;
        String path;
        while (!node_stack.isEmpty()) {
            node = node_stack.pollLast();
            path = path_stack.pollLast();
            if ((node.left == null) && (node.right == null))
                paths.add(path);
            if (node.left != null) {
                node_stack.add(node.left);
                path_stack.add(path + "->" + Integer.toString(node.left.val));
            }
            if (node.right != null) {
                node_stack.add(node.right);
                path_stack.add(path + "->" + Integer.toString(node.right.val));
            }
        }
        return paths;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，每个节点只会被访问一次。
- 空间复杂度：$O(N)$，在最坏情况下，队列中有 `N` 个节点。

