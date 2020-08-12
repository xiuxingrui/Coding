# [LeetCode105.从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
## 题目描述
根据一棵树的前序遍历与中序遍历构造二叉树。

注意:你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

## 题解
前序遍历数组的第 1 个数（索引为 0）的数一定是二叉树的根结点，于是可以在中序遍历中找这个根结点的索引，然后把“前序遍历数组”和“中序遍历数组”分为两个部分，就分别对应二叉树的左子树和右子树，分别递归完成就可以了。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812142639.png)

下面是一个具体的例子，演示了如何计算数组子区间的边界：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812142712.png)

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

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder==null||preorder.length==0||inorder==null||inorder.length==0){
            return null;
        }
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
     * @param inorder  二叉树后序遍历结果
     * @param inLeft   二叉树后序遍历结果的左边界
     * @param inRight  二叉树后序遍历结果的右边界
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
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder==null||preorder.length==0||inorder==null||inorder.length==0){
            return null;
        }
        HashMap<Integer,Integer> map=new HashMap<>();
        int preLen=preorder.length;
        int inLen=inorder.length;
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
     * @param inorder  二叉树后序遍历结果
     * @param inLeft   二叉树后序遍历结果的左边界
     * @param inRight  二叉树后序遍历结果的右边界
     * @return 二叉树的根结点
     */
    public TreeNode buildTreeHelper(int[] preorder, int preLeft, int preRight,
                               int[] inorder, int inLeft, int inRight,HashMap<Integer,Integer> map) {
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


