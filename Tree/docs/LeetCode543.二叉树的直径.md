# [LeetCode543.二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)
## 题解
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

注意：两结点之间的路径长度是以它们之间边的数目表示。
### 示例
```
          1
         / \
        2   3
       / \     
      4   5    
```
返回 3, 它的长度是路径 `[4,2,1,3]` 或者 `[5,2,1,3]`。

## 题解
首先我们知道一条路径的长度为该路径经过的节点数减一，所以求直径（即求路径长度的最大值）等效于求路径经过节点数的最大值减一。

而任意一条路径均可以被看作由某个节点为起点，从其左儿子和右儿子向下遍历的路径拼接得到。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200908154840.png)


如图我们可以知道路径 `[9, 4, 2, 5, 7, 8]` 可以被看作以 222 为起点，从其左儿子向下遍历的路径 `[2, 4, 9]` 和从其右儿子向下遍历的路径 `[2, 5, 7, 8]` 拼接得到。

假设我们知道对于该节点的左儿子向下遍历经过最多的节点数 `L` （即以左儿子为根的子树的深度） 和其右儿子向下遍历经过最多的节点数 `R` （即以右儿子为根的子树的深度），那么以该节点为起点的路径经过节点数的最大值即为 `L+R+1`

我们记节点 `node` 为起点的路径经过节点数的最大值为$d_{\text {node}}$，那么二叉树的直径就是所有节点$d_{\text {node}}$的最大值减一。

最后的算法流程为：我们定义一个递归函数 `depth(node)` 计算$d_{\text {node}}$，函数返回该节点为根的子树的深度。先递归调用左儿子和右儿子求得它们为根的子树的深度 `L` 和 `R` ，则该节点为根的子树的深度即为

`max(L,R)+1max(L,R)+1`

该节点$d_{\text {node}}$的值为

`L+R+1`

递归搜索每个节点并设一个全局变量 `ans` 记录的最大值$d_{\text {node}}$，最后返回 `ans-1` 即为树的直径。


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
    int max=Integer.MIN_VALUE;
    public int diameterOfBinaryTree(TreeNode root) {
        if(root==null) return 0;
        helper(root);
        return max;
    }
    public int helper(TreeNode root){
        if(root==null) return 0;
        int leftMax=helper(root.left);
        int rightMax=helper(root.right);
        max=Math.max(max,leftMax+rightMax);
        return 1+Math.max(leftMax,rightMax);
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 为二叉树的节点数，即遍历一棵二叉树的时间复杂度，每个结点只被访问一次。
- 空间复杂度：$O(Height)$，其中 `Height` 为二叉树的高度。由于递归函数在递归过程中需要为每一层递归函数分配栈空间，所以这里需要额外的空间且该空间取决于递归的深度，而递归的深度显然为二叉树的高度，并且每次递归调用的函数里又只用了常数个变量，所以所需空间复杂度为 $O(Height)$ 。
