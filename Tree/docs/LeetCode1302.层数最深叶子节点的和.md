# [LeetCode1302.层数最深叶子节点的和](https://leetcode-cn.com/problems/deepest-leaves-sum/)
## 题目描述
给你一棵二叉树，请你返回层数最深的叶子节点的和。

- 树中节点数目在 1 到 10^4 之间。
- 每个节点的值在 1 到 100 之间。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201026103317.png)
```
输入：root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
输出：15
```
## 题解
### 解法一
`DFS`:
我们可以使用深度优先搜索的方法解决这个问题。

我们从根节点开始进行搜索，在搜索的同时记录当前节点的深度 `dep`。我们维护两个全局变量 `maxdep` 和 `total`，其中 `maxdep` 表示搜索到的节点的最大深度，`total` 表示搜索到的深度等于 `maxdep` 的节点的权值之和。

当我们搜索到一个新的节点 `x` 时，会有以下三种情况：

- 节点 `x` 的深度 `dep` 小于 `maxdep`，那么我们可以忽略节点 `x`，继续进行搜索；
- 节点 `x` 的深度 `dep` 等于 `maxdep`，那么我们将节点 `x` 的权值添加到 `total` 中；
- 节点 `x` 的深度 `dep` 大于 `maxdep`，此时我们找到了一个深度更大的节点，因此需要将 `maxdep` 置为 `dep`，并将 `total` 置为节点 `x` 的权值。

在深度优先搜索结束之后，深度最大的叶子节点的权值之和即存储在 `total` 中。

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
    int ans=0;
    int maxL=1;
    public int deepestLeavesSum(TreeNode root) {
        preOrder(root,1);
        return ans;
    }
    public void preOrder(TreeNode root,int level){
        if(root==null){
            return;
        }
        if(level>maxL){
            maxL=level;
            ans=0;
        }
        if(level==maxL){
            ans+=root.val;
        }
        preOrder(root.left,level+1);
        preOrder(root.right,level+1);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是树中的节点个数。
- 空间复杂度：$O(H)$，其中 `H` 是树的高度（最大深度）。
### 解法二
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
    public int deepestLeavesSum(TreeNode root) {
        Queue<TreeNode> queue=new LinkedList<>();
        queue.offer(root);
        int ans=0;
        while(!queue.isEmpty()){
            ans=0;
            int size=queue.size();//因为size动态变化，所以必须先用一个变量存储容量
            for(int i=0;i<size;i++){
                TreeNode temp=queue.poll();
                if(temp.left==null&&temp.right==null){
                    ans+=temp.val;
                }
                if(temp.left!=null){
                    queue.offer(temp.left);
                }
                if(temp.right!=null){
                    queue.offer(temp.right);
                }
            }
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是树中的节点个数。
- 空间复杂度：$O(N)$。

