# [LeetCode1008.先序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/)
## 题目描述
返回与给定先序遍历 `preorder` 相匹配的二叉搜索树（`binary search tree`）的根结点。
(回想一下，二叉搜索树是二叉树的一种，其每个节点都满足以下规则，对于 `node.left` 的任何后代，值总 `< node.val`，而 `node.right` 的任何后代，值总 `> node.val`。此外，先序遍历首先显示节点的值，然后遍历 `node.left`，接着遍历 `node.right`。）

1. `1 <= preorder.length <= 100`
2. 先序 `preorder` 中的值是不同的。

### 示例
```
输入：[8,5,1,7,10,12]
输出：[8,5,10,1,7,null,12]
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812160528.png)
## 题解
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {

    public TreeNode bstFromPreorder(int[] preorder) {
        if(preorder==null||preorder.length==0){
            return null;
        }
        int[] inorder=Arrays.copyOf(preorder,preorder.length);
        Arrays.sort(inorder);
        int preLen=preorder.length;
        int inLen=inorder.length;
        return buildTreeHelper(preorder, 0, preLen - 1, inorder, 0, inLen - 1);
    }


    /**
     * 使用数组 preorder 在索引区间 [preLeft, preRight] 中的所有元素
     * 和数组 inorder 在索引区间 [inLeft, inRight] 中的所有元素构造二叉树
     *
     * @param preorder 二叉树前序遍历结果
     * @param preLeft  二叉树前序遍历结果的左边界
     * @param preRight 二叉树前序遍历结果的右边界
     * @param inorder  二叉树中序遍历结果
     * @param inLeft   二叉树中序遍历结果的左边界
     * @param inRight  二叉树中序遍历结果的右边界
     * @return 二叉树的根结点
     */
    public TreeNode buildTreeHelper(int[] preorder, int preLeft, int preRight,
                               int[] inorder, int inLeft, int inRight) {
        // 因为是递归调用的方法，按照国际惯例，先写递归终止条件
        if (preLeft > preRight || inLeft > inRight) {
            return null;
        }
        // 先序遍历的起点元素很重要
        int pivot = preorder[preLeft];
        TreeNode root = new TreeNode(pivot);
        int pivotIndex = inLeft;
        // 严格上说还要做数组下标是否越界的判断 pivotIndex < inRight
        while (inorder[pivotIndex] != pivot) {
            pivotIndex++;
        }
        root.left = buildTreeHelper(preorder, preLeft + 1, pivotIndex - inLeft + preLeft,
                inorder, inLeft, pivotIndex - 1);
        root.right = buildTreeHelper(preorder, pivotIndex - inLeft + preLeft + 1, preRight,
                inorder, pivotIndex + 1, inRight);
        return root;
    }
}
```
可以将中序遍历的值和索引存在一个哈希表中，这样就可以一下子找到根结点在中序遍历数组中的索引。
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {

    public TreeNode bstFromPreorder(int[] preorder) {
        if(preorder==null||preorder.length==0){
            return null;
        }
        int[] inorder=Arrays.copyOf(preorder,preorder.length);
        Arrays.sort(inorder);
        int preLen=preorder.length;
        int inLen=inorder.length;
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<inLen;i++){
            map.put(inorder[i],i);
        }
        return buildTreeHelper(preorder, 0, preLen - 1, inorder, 0, inLen - 1,map);
    }


    /**
     * 使用数组 preorder 在索引区间 [preLeft, preRight] 中的所有元素
     * 和数组 inorder 在索引区间 [inLeft, inRight] 中的所有元素构造二叉树
     *
     * @param preorder 二叉树前序遍历结果
     * @param preLeft  二叉树前序遍历结果的左边界
     * @param preRight 二叉树前序遍历结果的右边界
     * @param inorder  二叉树中序遍历结果
     * @param inLeft   二叉树中序遍历结果的左边界
     * @param inRight  二叉树中序遍历结果的右边界
     * @return 二叉树的根结点
     */
    public TreeNode buildTreeHelper(int[] preorder, int preLeft, int preRight,
                               int[] inorder, int inLeft, int inRight,HashMap<Integer,Integer> map){
        // 因为是递归调用的方法，按照国际惯例，先写递归终止条件
        if (preLeft > preRight || inLeft > inRight) {
            return null;
        }
        // 先序遍历的起点元素很重要
        int pivot = preorder[preLeft];
        TreeNode root = new TreeNode(pivot);
        int pivotIndex = map.get(pivot);
        root.left = buildTreeHelper(preorder, preLeft + 1, pivotIndex - inLeft + preLeft,
                inorder, inLeft, pivotIndex - 1,map);
        root.right = buildTreeHelper(preorder, pivotIndex - inLeft + preLeft + 1, preRight,
                inorder, pivotIndex + 1, inRight,map);
        return root;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是树中的节点个数。
- 空间复杂度：$O(n)$，除去返回的答案需要的 $O(n)$ 空间之外，我们还需要使用 $O(n)$ 的空间存储哈希映射，以及 $O(h)$（其中 `h` 是树的高度）的空间表示递归时栈空间。这里 `h<n`，所以总空间复杂度为 $O(n)$。


