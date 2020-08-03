# [LeetCode108.将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
## 题目描述
将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

### 示例
```java
给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```
## 题解
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
    public TreeNode sortedArrayToBST(int[] nums) {
        int len=nums.length;
        return constructTree(nums,0,len-1);
    }
    public TreeNode constructTree(int[] nums,int s,int e){
        if(s>e){
            return null;
        }
        int mid=s+(e-s)/2;
        TreeNode root=new TreeNode(nums[mid]);
        root.left=constructTree(nums,s,mid-1);
        root.right=constructTree(nums,mid+1,e);
        return root;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是数组的长度。每个数字只访问一次。
- 空间复杂度：$O(logn)$，其中 `n` 是数组的长度。空间复杂度不考虑返回值，因此空间复杂度主要取决于递归栈的深度，递归栈的深度是 $O(logn)$。