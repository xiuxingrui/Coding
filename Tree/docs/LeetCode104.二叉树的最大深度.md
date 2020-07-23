# [LeetCode104.二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
## 题目描述
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
### 示例
给定二叉树 `[3,9,20,null,null,15,7]`，

```   
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。

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
class Solution {
    public int maxDepth(TreeNode root) {
        if(root==null){
            return 0;
        }
        return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
}
```
#### 复杂度分析
- 时间复杂度：我们每个结点只访问一次，因此时间复杂度为 $O(N)$，其中 `N` 是结点的数量。
- 空间复杂度：在最糟糕的情况下，树是完全不平衡的，例如每个结点只剩下左子结点，递归将会被调用 `N` 次（树的高度），因此保持调用栈的存储将是 $O(N)$。但在最好的情况下（树是完全平衡的），树的高度将是$log(N)$。因此，在这种情况下的空间复杂度将是 $O(log(N))$。

### 解法二:栈
写法一:
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
        /**
     * DFS迭代实现二叉树最大深度
     * 时间复杂度O(n)
     * 空间复杂度:线性表最差O(n)、二叉树完全平衡最好O(logn)
     *
     * @param root 根节点
     * @return 最大深度
     */
    public  int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Deque<Pair<TreeNode, Integer>> stack = new LinkedList<>();
        stack.push(new Pair<>(root, 1));
        int maxDepth = 0;
        //DFS实现前序遍历，每个节点记录其所在深度
        while (!stack.isEmpty()) {
            Pair<TreeNode, Integer> pair = stack.pop();
            TreeNode node = pair.getKey();
            int depth=pair.getValue();
            //DFS过程不断比较更新最大深度
            maxDepth = Math.max(maxDepth, depth);
            //当前节点的子节点入栈，同时深度+1
            if (node.right != null) {
                stack.push(new Pair<>(node.right, depth + 1));
            }
            if (node.left != null) {
                stack.push(new Pair<>(node.left, depth + 1));
            }
        }
        return maxDepth;
    }
}
```
写法二
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
    public int maxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        Deque<TreeNode> stack = new LinkedList<>();
        Deque<Integer> value = new LinkedList<>();
        stack.push(root);
        value.push(1);
        int depth = 0;
        while(!stack.isEmpty()) {
            TreeNode node = stack.pop();
            int temp = value.pop();
            depth = Math.max(temp, depth);
            if(node.left != null) {
                stack.push(node.left);
                value.push(temp+1);
            }
            if(node.right != null) {
                stack.push(node.right);
                value.push(temp+1);
            }
        }
        return depth;
    }
}
```
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
    public int maxDepth(TreeNode root){
        
        if(root==null) return 0;
        
        int depth = 0;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while( !queue.isEmpty() ){
            depth++;
            int size = queue.size();//注意，这里必须先拿到size!(size是上一层的node个数)
            for(int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                if(node.left != null)
                    queue.offer(node.left);
                if(node.right != null)
                    queue.offer(node.right);
            }
        }
        
        return depth;
    }
}
```