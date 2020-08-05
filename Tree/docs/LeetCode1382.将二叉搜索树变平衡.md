# [LeetCode1382.将二叉搜索树变平衡](https://leetcode-cn.com/problems/balance-a-binary-search-tree/)
## 题目描述
给你一棵二叉搜索树，请你返回一棵 平衡后 的二叉搜索树，新生成的树应该与原来的树有着相同的节点值。

如果一棵二叉搜索树中，每个节点的两棵子树高度差不超过 1 ，我们就称这棵二叉搜索树是 平衡的 。

如果有多种构造方法，请你返回任意一种。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200803180640.png)

- 树节点的数目在 1 到 10^4 之间。
- 树节点的值互不相同，且在 1 到 10^5 之间。
### 示例
```java
输入：root = [1,null,2,null,3,null,4,null,null]
输出：[2,1,3,null,null,null,4]
解释：这不是唯一的正确答案，[3,1,4,null,2,null,null] 也是一个可行的构造方案。
```
## 题解
### 解法一:手撕AVL
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
    public TreeNode balanceBST(TreeNode root) {
        List<Integer> list=new ArrayList<>();
        inorder(root,list);
        return balanceBSTHelper(list,0,list.size()-1);
    }
    public void inorder(TreeNode root,List<Integer> list){
        if(root==null){
            return;
        }
        inorder(root.left,list);
        list.add(root.val);
        inorder(root.right,list);
    }
    public TreeNode balanceBSTHelper(List<Integer> list,int start,int end){
        if(start>end){
            return null;
        }
        int mid=start+(end-start)/2;
        TreeNode root=new TreeNode(list.get(mid));
        root.left=balanceBSTHelper(list,start,mid-1);
        root.right=balanceBSTHelper(list,mid+1,end);
        return root;
    }
}
```

