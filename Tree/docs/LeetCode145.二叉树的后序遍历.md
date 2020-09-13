# [LeetCode145.二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)
## 题目描述
给定一个二叉树，返回它的 后序 遍历。
### 示例
```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
```
## 题解
### 解法一:递归
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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list=new ArrayList<>();
        postOrder(root,list);
        return list;
    }
    public void postOrder(TreeNode root,List<Integer> list){
        if(root==null) return;
        postOrder(root.left,list);
        postOrder(root.right,list);
        list.add(root.val);
    }
}
```
### 解法二:常规迭代
区别于中序遍历和先序遍历，后序遍历的非递归形式会相对难一些。

原因就是，当遍历完某个根节点的左子树，回到根节点的时候，对于中序遍历和先序遍历可以把当前根节点从栈里弹出，然后转到右子树。举个例子，
```
     1
    / \
   2   3
  / \
 4   5
```
当遍历完 2,4,5 的时候，回到 1 之后我们就可以把 1 弹出，然后通过 1 到达右子树继续遍历。

而对于后序遍历，当我们到达 1 的时候并不能立刻把 1 弹出，因为遍历完右子树，我们还需要将这个根节点加入到 `list` 中。

所以我们就需要判断是从左子树到的根节点，还是右子树到的根节点。

如果是从左子树到的根节点，此时应该转到右子树。如果是从右子树到的根节点，那么就可以把当前节点弹出，并且加入到 `list` 中。

当然，如果是从左子树到的根节点，此时如果根节点的右子树为 `null`， 此时也可以把当前节点弹出，并且加入到 `list` 中。

通过记录上一次遍历的节点,判断当前是从左子树到的根节点还是右子树到的根节点,如果当前节点的右节点和上一次遍历的节点相同，那就表明当前是从右节点过来的了。

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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ans=new ArrayList<>();
        Deque<TreeNode> stack=new LinkedList<>();
        TreeNode cur = root;
        TreeNode last = null;
        while(!stack.isEmpty()||cur!=null){
            while(cur!=null){
                stack.offerFirst(cur);
                cur=cur.left;
            }
            TreeNode temp=stack.peek();
            if(temp.right!=null&&temp.right!=last){
                cur=temp.right;
            }else{
                ans.add(temp.val);
                last=temp;
                stack.pollFirst();
            }
        }
        return ans;
    }
}
```
### 解法三:巧妙迭代1
首先我们知道前序遍历的非递归形式会比后序遍历好理解些，那么我们能实现后序遍历 -> 前序遍历的转换吗？

后序遍历的顺序是 左 -> 右 -> 根。

前序遍历的顺序是 根 -> 左 -> 右，左右其实是等价的，所以我们也可以轻松的写出 根 -> 右 -> 左 的代码。

然后把 根 -> 右 -> 左 逆序，就是 左 -> 右 -> 根，也就是后序遍历了。

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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            if (cur != null) {
                list.add(cur.val);
                stack.offerFirst(cur);
                cur = cur.right; // 考虑右子树
            } else {
                // 节点为空，就出栈
                cur = stack.pullFirst();
                // 考虑左子树
                cur = cur.left;
            }
        }
        Collections.reverse(list);
        return list;
    }
}
```
或者:
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
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> list = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            if (cur != null) {
                list.addFirst(cur.val);
                stack.offerFirst(cur);
                cur = cur.right; // 考虑左子树
            } else {
                // 节点为空，就出栈
                cur = stack.pullFirst();
                // 考虑右子树
                cur = cur.left;
            }
        }
        return list;
    }
}
```
### 解法四:巧妙迭代2
下图中的顶点按照访问的顺序编号，按照 1-2-3-4-5 的顺序来比较不同的策略。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200811235625.png)

从根节点开始依次迭代，弹出栈顶元素输出到输出列表中，然后依次压入它的所有孩子节点，按照从上到下、从左至右的顺序依次压入栈中。

因为深度优先搜索后序遍历的顺序是从下到上、从左至右，所以需要将输出列表逆序输出。
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
  public List<Integer> postorderTraversal(TreeNode root) {
    Deque<TreeNode> stack = new LinkedList<>();
    LinkedList<Integer> output = new LinkedList<>();
    if (root == null) {
      return output;
    }
    stack.offerFirst(root);
    while (!stack.isEmpty()) {
      TreeNode node = stack.pullFirst();
      output.addFirst(node.val);
      if (node.left != null) {
        stack.offerFirst(node.left);
      }
      if (node.right != null) {
        stack.offerFirst(node.right);
      }
    }
    return output;
  }
}
```
#### 复杂度分析

- 时间复杂度：访问每个节点恰好一次，因此时间复杂度为 $O(N)$，其中 `N` 是节点的个数，也就是树的大小。
- 空间复杂度：取决于树的结构，最坏情况需要保存整棵树，因此空间复杂度为 $O(N)$。








