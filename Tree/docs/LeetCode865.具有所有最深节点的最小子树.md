# [LeetCode865.具有所有最深节点的最小子树](https://leetcode-cn.com/problems/smallest-subtree-with-all-the-deepest-nodes/)
## 题目描述
给定一个根为 `root` 的二叉树，每个节点的深度是 该节点到根的最短距离 。

如果一个节点在 整个树 的任意节点之间具有最大的深度，则该节点是 最深的 。

一个节点的 子树 是该节点加上它的所有后代的集合。

返回能满足 以该节点为根的子树中包含所有最深的节点 这一条件的具有最大深度的节点。

- 树中节点的数量介于 1 和 500 之间。
- `0 <= Node.val <= 500`
- 每个节点的值都是独一无二的。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201203165933.png)
```
输入：root = [3,5,1,6,2,0,8,null,null,7,4]
输出：[2,7,4]
解释：
我们返回值为 2 的节点，在图中用黄色标记。
在图中用蓝色标记的是树的最深的节点。
注意，节点 5、3 和 2 包含树中最深的节点，但节点 2 的子树最小，因此我们返回它。
```
```
输入：root = [1]
输出：[1]
解释：根节点是树中最深的节点。
```
```
输入：root = [0,1,3,null,2]
输出：[2]
解释：树中最深的节点为 2 ，有效子树为节点 2、1 和 0 的子树，但节点 2 的子树最小。
```
## 题解
最直白的做法，先做一次深度优先搜索标记所有节点的深度来找到最深的节点，再做一次深度优先搜索用回溯法找最小子树。定义第二次深度优先搜索方法为 `answer(node)`，每次递归有以下四种情况需要处理：

- 如果 `node` 没有左右子树，返回 `node`。
- 如果 `node` 左右子树的后代中都有最深节点，返回 `node`。
- 如果只有左子树或右子树中有且拥有所有的最深节点，返回这棵子树的根节点（即 `node` 的左/右孩子）。
- 否则，当前子树中不存在答案。

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
    int maxDepth=-1;
    public TreeNode subtreeWithAllDeepest(TreeNode root) {
        getMaxDepth(root,0);
        return answer(root,0);
    }
    public void getMaxDepth(TreeNode root,int depth){
        if(root==null){
            return;
        }
        maxDepth=Math.max(maxDepth,depth);
        getMaxDepth(root.left,depth+1);
        getMaxDepth(root.right,depth+1);
    }
    public TreeNode answer(TreeNode root,int depth){
        if(root==null||depth==maxDepth){
            return root;
        }
        TreeNode left=answer(root.left,depth+1);
        TreeNode right=answer(root.right,depth+1);
        if(left==null){
            return right;
        }
        if(right==null){
            return left;
        }
        return root;
    }
}
```
### 复杂度分析
- 时间复杂度： $O(N)$，其中 `N` 为树的大小。
- 空间复杂度： $O(N)$。