# [LeetCode863.二叉树中所有距离为K的结点](https://leetcode-cn.com/problems/all-nodes-distance-k-in-binary-tree/)
## 题目描述
给定一个二叉树（具有根结点 `root`）， 一个目标结点 `target` ，和一个整数值 `K` 。

返回到目标结点 `target` 距离为 `K` 的所有结点的值的列表。 答案可以以任何顺序返回。

- 给定的树是非空的。
- 树上的每个结点都具有唯一的值 `0 <= node.val <= 500` 。
- 目标结点 `target` 是树上的结点。
- `0 <= K <= 1000`.
### 示例
```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2
输出：[7,4,1]
解释：
所求结点为与目标结点（值为 5）距离为 2 的结点，
值分别为 7，4，以及 1
```
## 题解
如果 `target` 节点在 `root` 节点的左子树中，且 `target` 节点深度为 3，那所有 `root` 节点右子树中深度为 `K - 3` 的节点到 `target` 的距离就都是 `K`。

深度优先遍历所有节点。定义方法 `dfs(node)`，这个函数会返回 `node` 到 `target` 的距离。在 `dfs(node)` 中处理下面四种情况：

- `如果 `node == target`，把子树中距离 `target` 节点距离为 `K` 的所有节点加入答案。
- 如果 `target` 在 `node` 左子树中，假设 `target` 距离 `node` 的距离为 `L+1`，找出右子树中距离 `target` 节点 `K - L - 1` 距离的所有节点加入答案。
- 如果 `target` 在 `node` 右子树中，跟在左子树中一样的处理方法。
- 如果 `target` 不在节点的子树中，不用处理。

实现的算法中，还会用到一个辅助方法 `subtree_add(node, dist)`，这个方法会将子树中距离节点 `node` `K - dist` 距离的节点加入答案。

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
    List<Integer> ans;
    TreeNode target;
    int K;
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        ans = new LinkedList();
        this.target = target;
        this.K = K;
        dfs(root);
        return ans;
    }

    // Return vertex distance from node to target if exists, else -1
    // Vertex distance: the number of vertices on the path from node to target
    public int dfs(TreeNode node) {
        if (node == null)
            return -1;
        else if (node == target) {
            subtree_add(node, 0);
            return 1;
        } else {
            int L = dfs(node.left), R = dfs(node.right);
            if (L != -1) {
                if (L == K) ans.add(node.val);
                subtree_add(node.right, L + 1);
                return L + 1;
            } else if (R != -1) {
                if (R == K) ans.add(node.val);
                subtree_add(node.left, R + 1);
                return R + 1;
            } else {
                return -1;
            }
        }
    }

    // Add all nodes 'K - dist' from the node to answer.
    public void subtree_add(TreeNode node, int dist) {
        if (node == null) return;
        if (dist == K)
            ans.add(node.val);
        else {
            subtree_add(node.left, dist + 1);
            subtree_add(node.right, dist + 1);
        }
    }
}
```
### 复杂度分析
- 时间复杂度： $O(N)$，其中 `N` 树的大小。
- 空间复杂度： $O(N)$。

