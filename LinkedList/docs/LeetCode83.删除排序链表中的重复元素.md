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
### 解法一:迭代1
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
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        ListNode dummyHead=new ListNode(0);
        dummyHead.next=head;
        ListNode tail=head;
        ListNode cur=head.next;
        tail.next=null;
        while(cur!=null){
            if(cur.val==tail.val){
                cur=cur.next;
            }else{
                tail.next=cur;
                tail=cur;
                cur=cur.next;
                tail.next=null;
            }
        }
        return dummyHead.next;
    }
}
```
### 解法二:迭代2
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
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        ListNode cur=head;
        while(cur!=null){
            ListNode temp=cur;
            while(cur.next!=null&&cur.next.val==temp.val){
                cur=cur.next;
            }
            temp.next=cur.next;
            cur=cur.next;
        }
        return head;
    }
}
```
### 解法三:迭代3
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
#### 复杂度分析

时间复杂度：$O(n)$，因为列表中的每个结点都检查一次以确定它是否重复，所以总运行时间为 $O(n)$，其中 $n$ 是列表中的结点数。

空间复杂度：$O(1)$，没有使用额外的空间。

### 解法四:递归
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
#### 复杂度分析

时间复杂度：$O(n)$

空间复杂度：$O(n)$