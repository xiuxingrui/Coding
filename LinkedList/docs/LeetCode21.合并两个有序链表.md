<!-- TOC -->

- [[LeetCode21.合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)](#leetcode21合并两个有序链表httpsleetcode-cncomproblemsmerge-two-sorted-lists)
    - [题目描述](#题目描述)
        - [示例](#示例)
    - [题解](#题解)
        - [解法一：迭代](#解法一迭代)
        - [解法二：递归](#解法二递归)

<!-- /TOC -->
# [LeetCode21.合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
## 题目描述
将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
### 示例
```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```
## 题解
### 解法一：迭代
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dum = new ListNode(0), cur = dum;
        while(l1 != null && l2 != null) {
            if(l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            }
            else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = l1 != null ? l1 : l2;
        return dum.next;
    }
}
```
### 解法二：递归
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) {
            return l2;
        }
        if(l2 == null) {
            return l1;
        }

        if(l1.val <=l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```
