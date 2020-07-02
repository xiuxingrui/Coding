# [剑指Offer36.二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)
## 题目描述
将一个 **二叉搜索树** 就地转化为一个 **已排序的双向循环链表** 。

对于双向循环列表，你可以将左右孩子指针作为双向循环链表的前驱和后继指针，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

特别地，我们希望可以 **就地** 完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中最小元素的指针。

### 示例1
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200702112719.png)
### 示例2
```
输入：root = [2,1,3]
输出：[1,2,3]
```
### 示例3
```
输入：root = []
输出：[]
解释：输入是空树，所以输出也是空链表。
```
### 示例4
```java
输入：root = [1]
输出：[1]
```
### 提示:
1. `-1000 <= Node.val <= 1000`
2. `Node.left.val < Node.val < Node.right.val`
3. `Node.val` 的所有值都是独一无二的
4. `0 <= Number of Nodes <= 2000`

## 题解
### 解法一：递归
```java
class Solution {
  // the smallest (first) and the largest (last) nodes
  Node first = null;
  Node last = null;

  public void helper(Node node) {
    if (node != null) {
      // left
      helper(node.left);
      // node 
      if (last != null) {
        // link the previous node (last)
        // with the current one (node)
        last.right = node;
        node.left = last;
      }
      else {
        // keep the smallest node
        // to close DLL later on
        first = node;
      }
      last = node;
      // right
      helper(node.right);
    }
  }

  public Node treeToDoublyList(Node root) {
    if (root == null) return null;

    helper(root);
    // close DLL
    last.right = first;
    first.left = last;
    return first;
  }
}
```
#### 复杂度分析
时间复杂度：$O(N)$，每个结点被处理一次。

空间复杂度：$O(N)$。需要树高度大小的递归栈，最好情况（平衡树）为 $O(logN)$，最坏情况下（完全不平衡）为 $O(N)$。
### 解法二:栈
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/

class Solution {
        Node first;
        Node pre;
    public Node treeToDoublyList(Node root) {
        if (root == null) return null;
        Deque<Node> stack = new LinkedList<>();
        while (!stack.isEmpty() || root != null){
            while (root != null) {
                stack.offerLast(root);
                root = root.left;
            }
            if (!stack.isEmpty()) {
                Node node = stack.pollLast();
                //first若为空则赋值，first只赋值一次
                if (first == null) {
                    first = node;
                }
                //pre为空则赋值
                if (pre == null) {
                    pre = node;
                }
                //否则将当前节点与pre连接，同时移动pre
                else {
                    pre.right = node;
                    node.left = pre;
                    pre = node;
                }
                root = node.right;
            }
        }
        //连接头尾
        first.left = pre;
        pre.right = first;
        return first;
    }
}
```