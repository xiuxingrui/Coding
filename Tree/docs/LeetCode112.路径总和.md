# [LeetCode112.路径总和](https://leetcode-cn.com/problems/path-sum/)
## 题目描述
给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

### 示例
给定如下二叉树，以及目标和 `sum = 22`，
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

```
返回 `true`, 因为存在目标和为 22 的根节点到叶子节点的路径 `5->4->11->2`。
## 题解
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
public class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null){
            return false;
        }
        if(root.left == null && root.right == null){
            return root.val == sum;
        }
        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
        
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是树的节点数。对每个节点访问一次。

- 空间复杂度：$O(H)$，其中 `H` 是树的高度。空间复杂度主要取决于递归时栈空间的开销，最坏情况下，树呈现链状，空间复杂度为 $O(N)$。平均情况下树的高度与节点数的对数正相关，空间复杂度为 $O(logN)$。
### 解法二:队列
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
    public boolean hasPathSum(TreeNode root, int sum) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        Queue<Integer> queueSum = new LinkedList<Integer>();
        if (root == null)
            return false;
        queue.offer(root);
        queueSum.offer(root.val); 
        while (!queue.isEmpty()) {
            int levelNum = queue.size(); // 当前层元素的个数
            for (int i = 0; i < levelNum; i++) {
                TreeNode curNode = queue.poll();
                int curSum = queueSum.poll();
                if (curNode != null) {
                    //判断叶子节点是否满足了条件
                    if (curNode.left == null && curNode.right == null && curSum == sum) { 
                        return true; 
                    }
                    //当前节点和累计的和加入队列
                    if (curNode.left != null) {
                        queue.offer(curNode.left);
                        queueSum.offer(curSum + curNode.left.val);
                    }
                    if (curNode.right != null) {
                        queue.offer(curNode.right);
                        queueSum.offer(curSum + curNode.right.val);
                    }
                }
            }
        }
        return false;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是树的节点数。对每个节点访问一次。
- 空间复杂度：$O(N)$，其中 `N` 是树的节点数。空间复杂度主要取决于队列的开销，队列中的元素个数不会超过树的节点数。
### 解法三:栈
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
    public boolean hasPathSum(TreeNode root, int sum) {
        Deque<TreeNode> stack = new LinkedList<>();
        Deque<Integer> stackSum = new LinkedList<>();
        TreeNode cur = root;
        int curSum = 0;
        while (cur != null || !stack.isEmpty()) {
            // 节点不为空一直压栈
            while (cur != null) {
                stack.push(cur);
                curSum += cur.val;
                stackSum.push(curSum);
                cur = cur.left; // 考虑左子树
            }
            // 节点为空，就出栈
            cur = stack.pop();
            curSum = stackSum.pop();
            //判断是否满足条件
            if (curSum == sum && cur.left == null && cur.right == null) {
                return true;
            }
            // 考虑右子树
            cur = cur.right;
        }
        return false;
    }
}
```
