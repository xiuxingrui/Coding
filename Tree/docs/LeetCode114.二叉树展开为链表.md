# [LeetCode114.二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)
## 题目描述
给定一个二叉树，原地将它展开为一个单链表。

### 示例
```
    1
   / \
  2   5
 / \   \
3   4   6
展开为:
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```
## 题解
### 解法一
1. 将左子树插入到右子树的地方
2. 将原来的右子树接到左子树的最右边节点
3. 考虑新的右子树的根节点，一直重复上边的过程，直到新的右子树为`null`

```java
    1
   / \
  2   5
 / \   \
3   4   6

//将 1 的左子树插入到右子树的地方
    1
     \
      2         5
     / \         \
    3   4         6        
//将原来的右子树接到左子树的最右边节点
    1
     \
      2          
     / \          
    3   4  
         \
          5
           \
            6
            
 //将 2 的左子树插入到右子树的地方
    1
     \
      2          
       \          
        3       4  
                 \
                  5
                   \
                    6   
        
 //将原来的右子树接到左子树的最右边节点
    1
     \
      2          
       \          
        3      
         \
          4  
           \
            5
             \
              6         
  
  ......
```
```java
public void flatten(TreeNode root) {
    while (root != null) { 
        //左子树为 null，直接考虑下一个节点
        if (root.left == null) {
            root = root.right;
        } else {
            // 找左子树最右边的节点
            TreeNode pre = root.left;
            while (pre.right != null) {
                pre = pre.right;
            } 
            //将原来的右子树接到左子树的最右边节点
            pre.right = root.right;
            // 将左子树插入到右子树的地方
            root.right = root.left;
            root.left = null;
            // 考虑下一个节点
            root = root.right;
        }
    }
}
```
### 解法二
题目其实就是将二叉树通过右指针，组成一个链表。

```java
1 -> 2 -> 3 -> 4 -> 5 -> 6
```
我们知道题目给定的遍历顺序其实就是先序遍历的顺序，所以我们能不能利用先序遍历的代码，每遍历一个节点，就将上一个节点的右指针更新为当前节点。

先序遍历的顺序是 `1 2 3 4 5 6`。

遍历到 2，把 1 的右指针指向 2。`1 -> 2 3 4 5 6`。

遍历到 3，把 2 的右指针指向 3。`1 -> 2 -> 3 4 5 6`。

一直进行下去似乎就解决了这个问题。但现实是残酷的，原因就是我们把 1 的右指针指向 2，那么 1 的原本的右孩子就丢失了，也就是 5 就找不到了。

解决方法的话，我们可以逆过来进行。

我们依次遍历 `6 5 4 3 2 1`，然后每遍历一个节点就将当前节点的右指针更新为上一个节点。

遍历到 5，把 5 的右指针指向 6。`6 <- 5 4 3 2 1`。

遍历到 4，把 4 的右指针指向 5。`6 <- 5 <- 4 3 2 1`。

```java
    1
   / \
  2   5
 / \   \
3   4   6
```
这样就不会有丢失孩子的问题了，因为更新当前的右指针的时候，当前节点的右孩子已经访问过了。

而 `6 5 4 3 2 1`的遍历顺序其实变形的后序遍历，遍历顺序是右子树->左子树->根节点。

递归写法:
```java
private TreeNode pre = null;

public void flatten(TreeNode root) {
    if (root == null)
        return;
    flatten(root.right);
    flatten(root.left);
    root.right = pre;
    root.left = null;
    pre = root;
}
```
迭代写法:
```java
public void flatten(TreeNode root) { 
    Stack<TreeNode> toVisit = new Stack<>();
    TreeNode cur = root;
    TreeNode pre = null;

    while (cur != null || !toVisit.isEmpty()) {
        while (cur != null) {
            toVisit.push(cur); // 添加根节点
            cur = cur.right; // 递归添加右节点
        }
        cur = toVisit.peek(); // 已经访问到最右的节点了
        // 在不存在左节点或者右节点已经访问过的情况下，访问根节点
        if (cur.left == null || cur.left == pre) {
            toVisit.pop(); 
            /**************修改的地方***************/
            cur.right = pre;
            cur.left = null;
            /*************************************/
            pre = cur;
            cur = null;
        } else {
            cur = cur.left; // 左节点还没有访问过就先访问左节点
        }
    } 
}
```
### 解法三
```java
class Solution {
    public void flatten(TreeNode root) {
        if(root == null){
            return ;
        }
        //将根节点的左子树变成链表
        flatten(root.left);
        //将根节点的右子树变成链表
        flatten(root.right);
        TreeNode temp = root.right;
        //把树的右边换成左边的链表
        root.right = root.left;
        //记得要将左边置空
        root.left = null;
        //找到树的最右边的节点
        while(root.right != null) root = root.right;
        //把右边的链表接到刚才树的最右边的节点
        root.right = temp;
    }
}
```
### 解法四
还有一种特殊的先序遍历，提前将右孩子保存到栈中，我们利用这种遍历方式就可以防止右孩子的丢失了。由于栈是先进后出，所以我们先将右节点入栈。

我们知道题目给定的遍历顺序其实就是先序遍历的顺序，所以我们可以利用先序遍历的代码，每遍历一个节点，就将上一个节点的右指针更新为当前节点。

先序遍历的顺序是 `1 2 3 4 5 6`。

遍历到 2，把 1 的右指针指向 2。`1 -> 2 3 4 5 6`。

遍历到 3，把 2 的右指针指向 3。`1 -> 2 -> 3 4 5 6`。


![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200802144116.gif)

因为我们用栈保存了右孩子，所以不需要担心右孩子丢失了。用一个 `prev` 变量保存上次遍历的节点。修改的代码如下：


```java
class Solution {
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        stack.push(root);
        TreeNode prev = null;
        while (!stack.isEmpty()) {
            TreeNode curr = stack.pop();
            //用栈作为辅助数据结构，从栈中弹出元素后
			//先将右节点放到栈中，再将左节点放入栈中，模拟前序遍历
            if (prev != null) {
                prev.left = null;
                prev.right = curr;
            }
            TreeNode left = curr.left, right = curr.right;
            //先放入右节点，再放入左边点，顺序不能反了
            if (right != null) {
                stack.push(right);
            }
            if (left != null) {
                stack.push(left);
            }
            prev = curr;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是二叉树的节点数。前序遍历的时间复杂度是 $O(n)$，前序遍历的同时对每个节点更新左右子节点的信息，更新子节点信息的时间复杂度是 $O(1)$，因此总时间复杂度是 $O(n)$。
- 空间复杂度：$O(n)$，其中 `n` 是二叉树的节点数。空间复杂度取决于栈的大小，栈内的元素个数不会超过 `n`。
### 解法五：前序遍历
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
class Solution {
    TreeNode cur;
    public TreeNode flatten(TreeNode root) {
        return helper(root);
    }
    public TreeNode helper(TreeNode root){
        if(root==null){
            return null;
        }
        TreeNode temp_left=root.left;
        TreeNode temp_right=root.right;
        cur=root;
        cur.left=null;
        cur.right=helper(temp_left);
        cur.right=helper(temp_right);
        return root;
    }
}
```
### 解法六:暴力法
递归:
```java
class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        preorderTraversal(root, list);
        int size = list.size();
        for (int i = 1; i < size; i++) {
            TreeNode prev = list.get(i - 1), curr = list.get(i);
            prev.left = null;
            prev.right = curr;
        }
    }

    public void preorderTraversal(TreeNode root, List<TreeNode> list) {
        if (root != null) {
            list.add(root);
            preorderTraversal(root.left, list);
            preorderTraversal(root.right, list);
        }
    }
}
```
迭代:
```java
class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode node = root;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                list.add(node);
                stack.push(node);
                node = node.left;
            }
            node = stack.pop();
            node = node.right;
        }
        int size = list.size();
        for (int i = 1; i < size; i++) {
            TreeNode prev = list.get(i - 1), curr = list.get(i);
            prev.left = null;
            prev.right = curr;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是二叉树的节点数。前序遍历的时间复杂度是 $O(n)$，前序遍历之后，需要对每个节点更新左右子节点的信息，时间复杂度也是 $O(n)$。
- 空间复杂度：$O(n)$，其中 `n` 是二叉树的节点数。空间复杂度取决于栈（递归调用栈或者迭代中显性使用的栈）和存储前序遍历结果的列表的大小，栈内的元素个数不会超过 `n`，前序遍历列表中的元素个数是 `n`。






