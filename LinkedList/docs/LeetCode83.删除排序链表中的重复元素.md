# [LeetCode83.删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
## 题目描述
给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
### 示例1
```
输入: 1->1->2
输出: 1->2
```
### 示例2
```
输入: 1->1->2->3->3
输出: 1->2->3
```
## 题解
### 题解一:迭代
```java
public ListNode deleteDuplicates(ListNode head) {
    ListNode current = head;
    while (current != null && current.next != null) {
        if (current.next.val == current.val) {
            current.next = current.next.next;
        } else {
            current = current.next;
        }
    }
    return head;
}
```
### 题解二:递归
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null||head.next==null) return head;
        if(head.val==head.next.val){
            head.next=deleteDuplicates(head.next).next;
        }
        else{
            head.next=deleteDuplicates(head.next);
        }
        return head;
    }
}
```