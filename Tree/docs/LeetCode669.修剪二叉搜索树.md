# [LeetCode669.修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)
## 题目描述
给定一个二叉搜索树，同时给定最小边界`L` 和最大边界 `R`。通过修剪二叉搜索树，使得所有节点的值在`[L, R]`中 `(R>=L)` 。你可能需要改变树的根节点，所以结果应当返回修剪好的二叉搜索树的新的根节点。

### 示例
```
输入: 
    1
   / \
  0   2

  L = 1
  R = 2

输出: 
    1
      \
       2
```
```
输入: 
    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

输出: 
      3
     / 
   2   
  /
 1
```
## 题解
```java
class Solution {
    public TreeNode trimBST(TreeNode root, int L, int R) {
        if (root == null) return root;
        if (root.val > R) return trimBST(root.left, L, R);
        if (root.val < L) return trimBST(root.right, L, R);

        root.left = trimBST(root.left, L, R);
        root.right = trimBST(root.right, L, R);
        return root;
    }
}
```
自己解法:
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
    public TreeNode trimBST(TreeNode root, int L, int R) {
        if(root==null){
            return null;
        }
        TreeNode leftNode=trimBST(root.left,L,R);
        TreeNode rightNode=trimBST(root.right,L,R);
        if(root.val<=R&&root.val>=L){
            root.left=leftNode;
            root.right=rightNode;
            return root;
        }else{
            if(leftNode==null){
                return rightNode;
            }else if(rightNode==null){
                return leftNode;
            }else{
                TreeNode cur=leftNode;
                while(cur.right!=null&&cur.right.right!=null){
                    cur=cur.right;
                }
                if(cur.right==null){
                    root.val=leftNode.val;
                    root.left=null;
                    root.right=rightNode;
                    return root;
                }else{
                    root.val=cur.right.val;
                    cur.right=null;
                    root.left=leftNode;
                    root.right=rightNode;
                    return root;
                }
            }
        }
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是给定的树的全部节点。我们最多访问每个节点一次。
- 空间复杂度：$O(N)$，即使我们没有明确使用任何额外的内存，在最糟糕的情况下，我们递归调用的栈可能与节点数一样大。
