# [LeetCode450.删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)
## 题目描述
给定一个二叉搜索树的根节点 `root` 和一个值 `key`，删除二叉搜索树中的 `key` 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

说明： 要求算法时间复杂度为 $O(h)$，`h` 为树的高度。

### 示例
```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]。

    5
   / \
  2   6
   \   \
    4   7
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
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root==null){
            return null;
        }
        if(root.val==key){
            if(root.left==null){
                return root.right;
            }else if(root.right==null){
                return root.left;
            }else{
                TreeNode temp=root.left;
                while(temp.right!=null){
                    temp=temp.right;
                }
                temp.right=root.right;
                return root.left;
            }
        }
        if(key<root.val){
            root.left=deleteNode(root.left,key);
        }else{
            root.right=deleteNode(root.right,key);
        }
        return root;
    }
}
```
自己的写法：
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
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root==null){
            return null;
        }
        if(root.val==key){
            if(root.left==null){
                return root.right;
            }else{
                TreeNode temp=root.left;
                while(temp.right!=null){
                    temp=temp.right;
                }
                root.val=temp.val;
                root.left=deleteNode(root.left,temp.val);
                return root;
            }
        }
        if(key<root.val){
            root.left=deleteNode(root.left,key);
        }else{
            root.right=deleteNode(root.right,key);
        }
        return root;
    }
}
```
### 复杂度分析

- 时间复杂度：$O(logN)$。在算法的执行过程中，我们一直在树上向左或向右移动。首先先用$O(H_{1})$的时间找到要删除的节点，$H_{1}$是从根节点到要删除节点的高度。然后删除节点需要$O(H_{2})$的时间，$H_{2}$指的是从要删除节点到替换节点的高度。由于$O(H_{1}+H_{2})=O(H)$，`H`是树的高度，若树是一个平衡树则$H=\log N$。
- 空间复杂度：$O(H)$，递归时堆栈使用的空间，`H` 是树的高度。

