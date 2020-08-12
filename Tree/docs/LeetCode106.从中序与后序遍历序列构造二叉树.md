# [LeetCode106.从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
## 题目描述
根据一棵树的中序遍历与后序遍历构造二叉树。

注意:你可以假设树中没有重复的元素。

例如，给出
```
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
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
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812145929.png)
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

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder==null||inorder.length==0||postorder==null||postorder.length==0){
            return null;
        }
        int inLen = inorder.length;
        int postLen = postorder.length;
        return buildTree(inorder, 0, inLen - 1, postorder, 0, postLen - 1);
    }

    /**
     * 使用中序遍历序列 inorder 的子区间 [inLeft, inRight]
     * 与后序遍历序列 postorder 的子区间 [postLeft, postRight] 构建二叉树
     *
     * @param inorder   中序遍历序列
     * @param inLeft    中序遍历序列的左边界
     * @param inRight   中序遍历序列的右边界
     * @param postorder 后序遍历序列
     * @param postLeft  后序遍历序列的左边界
     * @param postRight 后序遍历序列的右边界
     * @return 二叉树的根结点
     */
    private TreeNode buildTree(int[] inorder, int inLeft, int inRight,
                               int[] postorder, int postLeft, int postRight) {
        if (inLeft > inRight || postLeft > postRight) {
            return null;
        }

        int pivot = postorder[postRight];
        int pivotIndex = inLeft;

        // 注意这里如果编写不当，有数组下标越界的风险
        while (inorder[pivotIndex] != pivot) {
            pivotIndex++;
        }
        TreeNode root = new TreeNode(pivot);
        root.left = buildTree(inorder, inLeft, pivotIndex - 1,
                postorder, postLeft, postRight - inRight + pivotIndex - 1);

        root.right = buildTree(inorder, pivotIndex + 1, inRight,
                postorder, postRight - inRight + pivotIndex, postRight - 1);
        return root;
    }
}
```
可以把中序遍历的值和索引放在一个哈希表中，就不需要通过遍历得到当前根结点在中序遍历中的位置了。
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

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder==null||inorder.length==0||postorder==null||postorder.length==0){
            return null;
        }
        int inLen = inorder.length;
        int postLen = postorder.length;
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<inLen;i++){
            map.put(inorder[i],i);
        }
        return buildTreeHelper(inorder, 0, inLen - 1, postorder, 0, postLen - 1,map);
    }

    /**
     * 使用中序遍历序列 inorder 的子区间 [inLeft, inRight]
     * 与后序遍历序列 postorder 的子区间 [postLeft, postRight] 构建二叉树
     *
     * @param inorder   中序遍历序列
     * @param inLeft    中序遍历序列的左边界
     * @param inRight   中序遍历序列的右边界
     * @param postorder 后序遍历序列
     * @param postLeft  后序遍历序列的左边界
     * @param postRight 后序遍历序列的右边界
     * @return 二叉树的根结点
     */
    public TreeNode buildTreeHelper(int[] inorder, int inLeft, int inRight,
                               int[] postorder, int postLeft, int postRight,HashMap<Integer,Integer> map) {
        if (inLeft > inRight || postLeft > postRight) {
            return null;
        }

        int pivot = postorder[postRight];
        TreeNode root = new TreeNode(pivot);
        int pivotIndex = map.get(pivot);
        root.left = buildTreeHelper(inorder, inLeft, pivotIndex - 1,
                postorder, postLeft, postRight - inRight + pivotIndex - 1,map);

        root.right = buildTreeHelper(inorder, pivotIndex + 1, inRight,
                postorder, postRight - inRight + pivotIndex, postRight - 1,map);
        return root;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是树中的节点个数。
- 空间复杂度：$O(n)$，除去返回的答案需要的 $O(n)$ 空间之外，我们还需要使用 $O(n)$ 的空间存储哈希映射，以及 $O(h)$（其中 `h` 是树的高度）的空间表示递归时栈空间。这里 `h<n`，所以总空间复杂度为 $O(n)$。