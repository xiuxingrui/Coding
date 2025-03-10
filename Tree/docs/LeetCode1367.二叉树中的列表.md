# [LeetCode1367.二叉树中的列表](https://leetcode-cn.com/problems/linked-list-in-binary-tree/)
## 题目描述
给你一棵以 `root` 为根的二叉树和一个 `head` 为第一个节点的链表。

如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 `head` 为首的链表中每个节点的值，那么请你返回 `True`，否则返回 `False` 。

一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200908164524.png)

```
输入：head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
解释：树中蓝色的节点构成了与链表对应的子路径。
```

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200908164548.png)

```
输入：head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
```

```
输入：head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：false
解释：二叉树中不存在一一对应链表的路径。
```
## 题解
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
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
    public boolean isSubPath(ListNode head, TreeNode root) {
        return (head!=null&&root!=null)&&(isEqual(head,root)||isSubPath(head,root.left)||isSubPath(head,root.right));
    }
    public boolean isEqual(ListNode head,TreeNode root){
        if(head==null){
            return true;
        }
        if(root==null||root.val!=head.val){
            return false;
        }
        return isEqual(head.next,root.left)||isEqual(head.next,root.right);
    }
}
```


