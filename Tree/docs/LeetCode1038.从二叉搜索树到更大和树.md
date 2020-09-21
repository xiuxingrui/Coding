# [LeetCode1038.从二叉搜索树到更大和树](https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/)
## 题目描述
给出二叉 搜索 树的根节点，该二叉树的节点值各不相同，修改二叉树，使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键 小于 节点键的节点。
- 节点的右子树仅包含键 大于 节点键的节点。
- 左右子树也必须是二叉搜索树。

- 树中的节点数介于 1 和 100 之间。
- 每个节点的值介于 0 和 100 之间。
- 给定的树为二叉搜索树。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200921130915.png)
```
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```
## 题解
### 解法一
二叉搜索树的中序遍历是一个单调递增的有序序列。如果我们反序地中序遍历该二叉搜索树，即可得到一个单调递减的有序序列。

本题中要求我们将每个节点的值修改为原来的节点值加上所有大于它的节点值之和。这样我们只需要反序中序遍历该二叉搜索树，记录过程中的节点值之和，并不断更新当前遍历到的节点的节点值，即可得到题目要求的累加树。

```java
class Solution {
    int sum = 0;

    public TreeNode convertBST(TreeNode root) {
        if (root != null) {
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是二叉搜索树的节点数。每一个节点恰好被遍历一次。
- 空间复杂度：$O(n)$，为递归过程中栈的开销，平均情况下为 $O(logn)$，最坏情况下树呈现链状，为 $O(n)$。

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
    int index=0;
    public TreeNode convertBST(TreeNode root) {
        if(root==null||root.left==null&&root.right==null){
            return root;
        }
        List<Integer> path=new ArrayList<>();
        inOrder(root,path);
        int[] preSum=new int[path.size()];
        preSum[preSum.length-1]=path.get(preSum.length-1);
        for(int i=preSum.length-2;i>=0;i--){
            preSum[i]=preSum[i+1]+path.get(i);
        }
        helper(root,preSum);
        return root;
    }
    public void inOrder(TreeNode root,List<Integer> path){
        if(root==null){
            return;
        }
        inOrder(root.left,path);
        path.add(root.val);
        inOrder(root.right,path);
    }
    public void helper(TreeNode root,int[] preSum){
        if(root==null){
            return;
        }
        helper(root.left,preSum);
        root.val=preSum[index++];
        helper(root.right,preSum);
    }
}
```