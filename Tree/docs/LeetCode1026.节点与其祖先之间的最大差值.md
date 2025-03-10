# [LeetCode1026.节点与其祖先之间的最大差值](https://leetcode-cn.com/problems/maximum-difference-between-node-and-ancestor/)
## 题目描述
给定二叉树的根节点 `root`，找出存在于 不同 节点 `A` 和 `B` 之间的最大值 `V`，其中 `V = |A.val - B.val|`，且 `A` 是 `B` 的祖先。

（如果 `A` 的任何子节点之一为 `B`，或者 `A` 的任何子节点是 `B` 的祖先，那么我们认为 `A` 是 `B` 的祖先）

- 树中的节点数在 2 到 5000 之间。
- `0 <= Node.val <= 10^5`
### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201201160633.png)
```
输入：root = [8,3,10,1,6,null,14,null,null,4,7,13]
输出：7
解释： 
我们有大量的节点与其祖先的差值，其中一些如下：
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
在所有可能的差值中，最大值 7 由 |8 - 1| = 7 得出。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201201160653.png)
```
输入：root = [1,null,2,null,0,3]
输出：3
```
## 题解
使用全局变量保存最大差值，进行`dfs`，每次递归查找当前路径的最大值和最小值，到达叶子节点时使用当前路径的最大值和最小值更新全局变量。
```java
class Solution {
    int res = Integer.MIN_VALUE;

    public int maxAncestorDiff(TreeNode root) {
        if (root == null) return 0;
        //如果当前节点没有子节点，则直接返回
        helper(root, root.val, root.val);
        return res;
    }

    /**
     * 每条从根节点到叶子节点的路径中的最大值和最小值，并求出差值更新全局变量
     */
    private void helper(TreeNode node, int max, int min) {
        if (node == null) return;
        max = Math.max(node.val, max);
        min = Math.min(node.val, min);
        //到达叶子节点，求最大差值
        if (node.left == null && node.right == null) {
            res = Math.max(res, Math.abs(max - min));
        }
        helper(node.left, max, min);
        helper(node.right, max, min);
    }
}
```