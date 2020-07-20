# [LeetCode199.二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)
## 题目描述
给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。
### 示例
```
输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```
## 题解
### 解法一:BFS
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
                if (i == size - 1) {  //将当前层的最后一个节点放入结果列表
                    res.add(node.val);
                }
            }
        }
        return res;
    }
}
```
#### 复杂度分析
- 时间复杂度 : $O(n)$。 每个节点最多进队列一次，出队列一次，因此广度优先搜索的复杂度为线性。

- 空间复杂度 : $O(n)$。每个节点最多进队列一次，所以队列长度最大不不超过 `n`，所以这里的空间代价为 $O(n)$。

### 解法二:DFS
我们按照 「根结点 -> 右子树 -> 左子树」 的顺序访问，就可以保证每层都是最先访问最右边的节点的。
```java
class Solution {
    List<Integer> res = new ArrayList<>();

    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0); // 从根节点开始访问，根节点深度是0
        return res;
    }

    private void dfs(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        // 先访问 当前节点，再递归地访问 右子树 和 左子树。res.size()起到记录层数的作用
        if (depth == res.size()) {   // 如果当前节点所在深度还没有出现在res里，说明在该深度下当前节点是第一个被访问的节点，因此将当前节点加入res中。
            res.add(root.val);
        }
        depth++;
        dfs(root.right, depth);
        dfs(root.left, depth);
```
#### 复杂度分析
- 时间复杂度 : $O(n)$。深度优先搜索最多访问每个结点一次，因此是线性复杂度。

- 空间复杂度 : $O(n)$。最坏情况下，栈内会包含接近树高度的结点数量，占用 $O(n)$的空间。
