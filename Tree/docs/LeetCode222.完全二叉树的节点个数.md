# [LeetCode222.完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)
## 题目描述
给出一个完全二叉树，求出该树的节点个数。

说明：

完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 `h` 层，则该层包含 `1~2^h` 个节点。

### 示例
```
输入: 
    1
   / \
  2   3
 / \  /
4  5 6

输出: 6
```
## 题解
### 解法一
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
    public int countNodes(TreeNode root) {
        if(root==null){
            return 0;
        }
        return countNodes(root.left)+countNodes(root.right)+1;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$。
- 空间复杂度：$(d)=O(logN)$，其中 `d` 指的是树的的高度，运行过程中堆栈所使用的空间。

### 解法二
首先需要明确完全二叉树的定义：它是一棵空树或者它的叶子节点只出在最后两层，若最后一层不满则叶子节点只在最左侧。

再来回顾一下满二叉的节点个数怎么计算，如果满二叉树的层数为`h`，则总节点数为：`2^h-1`.

那么我们来对 `root` 节点的左右子树进行高度统计，分别记为 `left` 和 `right`，有以下两种结果：

- `left == right`。这说明，左子树一定是满二叉树，因为节点已经填充到右子树了，左子树必定已经填满了。所以左子树的节点总数我们可以直接得到，是 `2^left - 1`，加上当前这个 `root` 节点，则正好是 `2^left`。再对右子树进行递归统计。
- `left != right`。说明此时最后一层不满，但倒数第二层已经满了，可以直接得到右子树的节点个数。同理，右子树节点+`root`节点，总数为 `2^right`。再对左子树进行递归查找。

关于如何计算二叉树的层数，可以利用下面的递归来算，当然对于完全二叉树，可以利用其特点，不用递归直接算，具体请参考最后的完整代码。
```java
public int countLevel(TreeNode root){
        if(root == null){
            return 0;
        }
        return Math.max(countLevel(root.left),countLevel(root.right)) + 1;
}
```
如何计算 `2^left`，最快的方法是移位计算，因为运算符的优先级问题，记得加括号哦。

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
    public int countNodes(TreeNode root) {
        if(root == null){
           return 0;
        } 
        int left = countLevel(root.left);
        int right = countLevel(root.right);
        if(left == right){
            return countNodes(root.right) + (1<<left);//return countNodes(root.right) + (int)Math.pow(2,left);
        }else{
            return countNodes(root.left) + (1<<right);
        }
    }
    public int countLevel(TreeNode root){
        int level = 0;
        while(root != null){
            level++;
            root = root.left;
        }
        return level;
    }
}
```
#### 复杂度分析
- 时间复杂度：$(d)=O(logN)$，其中 `d` 指的是树的的高度。
- 空间复杂度：$(d)=O(logN)$，其中 `d` 指的是树的的高度，运行过程中堆栈所使用的空间。
