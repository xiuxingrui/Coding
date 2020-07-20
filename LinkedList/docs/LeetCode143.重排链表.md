# [LeetCode143.重排链表](https://leetcode-cn.com/problems/reorder-list/)
## 题目描述
给定一个单链表，

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200718213526.png)

将其重新排列后变为：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200718213544.png)

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

### 示例
```
给定链表 1->2->3->4, 重新排列为 1->4->2->3.

给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
```
## 题解
### 解法一
此题目为2019年计算机统考408真题，题目要求空间复杂度为$O(1)$，不能借助栈
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public void reorderList(ListNode head) {
    if (head == null || head.next == null || head.next.next == null) {
        return;
    }
    //找中点，链表分成两个
    ListNode slow = head;
    ListNode fast = head;
    //中间节点有两个时，返回第一个节点的写法如下
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    ListNode newHead = slow.next;
    slow.next = null;
    
    //第二个链表倒置
    newHead = reverseList(newHead);
    
    //链表节点依次连接
    while (newHead != null) {
        ListNode temp = newHead.next;
        newHead.next = head.next;
        head.next = newHead;
        head = newHead.next;
        newHead = temp;
    }
}
    public ListNode reverseList(ListNode head) {
        ListNode prev=null;
        while (head != null) {
            ListNode temp = head.next;
            head.next = prev;
            prev = head;
            head = temp;
        }
        return prev;
    }
}
```
### 解法二
```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) return;
        Deque<ListNode> stack = new LinkedList<>();
        ListNode p = head;
        while (p != null) {
            stack.push(p);
            p = p.next;
        }
        int n = stack.size();
        int count = (n-1)/ 2;
        p = head;
        while (count != 0) {
            ListNode tmp = stack.pop();
            tmp.next = p.next;
            p.next = tmp;
            p = tmp.next;
            --count;
        }
        stack.pop().next = null;
    }
}
```
### 解法三
```java
public void reorderList(ListNode head) {

    if (head == null || head.next == null || head.next.next == null) {
        return;
    }
    int len = 0;
    ListNode h = head;
    //求出节点数
    while (h != null) {
        len++;
        h = h.next;
    }

    reorderListHelper(head, len);
}

private ListNode reorderListHelper(ListNode head, int len) {
    if (len == 1) {
        ListNode outTail = head.next;
        head.next = null;
        return outTail;
    }
    if (len == 2) {
        ListNode outTail = head.next.next;
        head.next.next = null;
        return outTail;
    }
    //得到对应的尾节点，并且将头结点和尾节点之间的链表通过递归处理
    ListNode tail = reorderListHelper(head.next, len - 2);
    ListNode subHead = head.next;//中间链表的头结点
    head.next = tail;
    ListNode outTail = tail.next;  //上一层 head 对应的 tail
    tail.next = subHead;
    return outTail;
}
```


