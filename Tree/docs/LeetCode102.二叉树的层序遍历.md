# [LeetCode102.二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
## 题目描述
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

### 示例
二叉树：`[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
返回其层序遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```
## 题解
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<List<Integer>>();
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        if(root!=null){
            queue.offer(root);
        }
        while (!queue.isEmpty()) {
            List<Integer> level = new ArrayList<Integer>();
            int currentLevelSize = queue.size();
            for (int i = 1; i <= currentLevelSize; ++i) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            ret.add(level);
        }
        
        return ret;
    }
}
```
### 复杂度分析

记树上所有节点的个数为 `n`。

- 时间复杂度：每个点进队出队各一次，故渐进时间复杂度为 $O(n)$。
- 空间复杂度：队列中元素的个数不超过 `n` 个，故渐进空间复杂度为 $O(n)$。
