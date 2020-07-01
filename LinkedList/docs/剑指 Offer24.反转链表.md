# [剑指Offer24.反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)
## 题目描述
反转一个单链表。
### 示例
```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```
## 题解
### 解法一:迭代
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```
### 解法二:递归
```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
```