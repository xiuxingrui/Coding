# [LeetCode889.根据前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)
## 题目描述
返回与给定的前序和后序遍历匹配的任何二叉树。`pre` 和 `post` 遍历中的值是不同的正整数。

提示：

1. `1 <= pre.length == post.length <= 30`
2. `pre[]` 和 `post[]` 都是 `1, 2, ..., pre.length` 的排列
3. 每个输入保证至少有一个答案。如果有多个答案，可以返回其中一个。

### 示例
```
输入：pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
输出：[1,2,3,4,5,6,7]
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
public class Solution {

    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        if(pre==null||pre.length==0||post==null||post.length==0){
            return null;
        }
        int preLen=pre.length;
        int postLen=post.length;
        return buildTree(pre, 0, preLen - 1, post, 0, postLen - 1);
    }


    /**
     * 使用数组 preorder 在索引区间 [preLeft, preRight] 中的所有元素
     * 和数组 postorder 在索引区间 [postLeft, postRight] 中的所有元素构造二叉树
     *
     * @param preorder 二叉树前序遍历结果
     * @param preLeft  二叉树前序遍历结果的左边界
     * @param preRight 二叉树前序遍历结果的右边界
     * @param postorder  二叉树后序遍历结果
     * @param postLeft   二叉树后序遍历结果的左边界
     * @param postRight  二叉树后序遍历结果的右边界
     * @return 二叉树的根结点
     */
    public TreeNode buildTree(int[] preorder, int preLeft, int preRight,
                               int[] postorder, int postLeft, int postRight) {
        // 因为是递归调用的方法，按照国际惯例，先写递归终止条件
        if (preLeft > preRight || postLeft > postRight) {
            return null;
        }
        // 先序遍历的起点元素很重要
        TreeNode root = new TreeNode(preorder[preLeft]);
        if(preLeft==preRight){
            root.left=null;
            root.right=null;
            return root;
        }
        int pivotIndex = postLeft;
        while (postorder[pivotIndex] != preorder[preLeft+1]) {
            pivotIndex++;
        }
        int lenLeft=pivotIndex-postLeft+1,lenRight=postRight-pivotIndex-1;
        root.left = buildTree(preorder, preLeft + 1, preLeft+lenLeft,
                postorder, postLeft, pivotIndex);
        root.right = buildTree(preorder, preLeft+lenLeft+ 1, preRight,
                postorder, pivotIndex + 1, postRight-1);
        return root;
    }
}
```
使用哈希优化:
```java
public class Solution {

    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        if(pre==null||pre.length==0||post==null||post.length==0){
            return null;
        }
        int preLen=pre.length;
        int postLen=post.length;
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<postLen;i++){
            map.put(post[i],i);
        }
        return buildTree(pre, 0, preLen - 1, post, 0, postLen - 1,map);
    }


    /**
     * 使用数组 preorder 在索引区间 [preLeft, preRight] 中的所有元素
     * 和数组 postorder 在索引区间 [postLeft, postRight] 中的所有元素构造二叉树
     *
     * @param preorder 二叉树前序遍历结果
     * @param preLeft  二叉树前序遍历结果的左边界
     * @param preRight 二叉树前序遍历结果的右边界
     * @param postorder  二叉树后序遍历结果
     * @param postLeft   二叉树后序遍历结果的左边界
     * @param postRight  二叉树后序遍历结果的右边界
     * @return 二叉树的根结点
     */
    public TreeNode buildTree(int[] preorder, int preLeft, int preRight,
                               int[] postorder, int postLeft, int postRight,HashMap<Integer,Integer> map) {
        // 因为是递归调用的方法，按照国际惯例，先写递归终止条件
        if (preLeft > preRight || postLeft > postRight) {
            return null;
        }
        // 先序遍历的起点元素很重要
        TreeNode root = new TreeNode(preorder[preLeft]);
        if(preLeft==preRight){
            root.left=null;
            root.right=null;
            return root;
        }
        int pivotIndex = map.get(preorder[preLeft+1]);
        int lenLeft=pivotIndex-postLeft+1,lenRight=postRight-pivotIndex-1;
        root.left = buildTree(preorder, preLeft + 1, preLeft+lenLeft,
                postorder, postLeft, pivotIndex,map);
        root.right = buildTree(preorder, preLeft+lenLeft+ 1, preRight,
                postorder, pivotIndex + 1, postRight-1,map);
        return root;
    }
}
```