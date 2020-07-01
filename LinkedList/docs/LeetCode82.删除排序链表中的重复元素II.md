# [LeetCode82.删除排序链表中的重复元素II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
## 题目描述
给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 **没有重复出现** 的数字。
### 示例1
```
输入: 1->2->3->3->4->4->5
输出: 1->2->5
```
### 示例2
```
输入: 1->1->1->2->3
输出: 2->3
```
## 题解
### 解法一:递归
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
        if (head == null)  return head;
        if (head.next != null && head.val == head.next.val) {
            while (head.next != null && head.val == head.next.val) {
                head = head.next;
            }
            return deleteDuplicates(head.next);
        }
        else head.next = deleteDuplicates(head.next);
        return head;    
    }
}
```
### 解法二:迭代
```java
public class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null)
            return head;
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;

        while (head.next != null && head.next.next != null) {
            if (head.next.val == head.next.next.val) {
                int val = head.next.val;
                while (head.next != null && head.next.val == val) {
                    head.next = head.next.next;
                }            
            } else {
                head = head.next;
            }
        }
        
        return dummy.next;
    }
}
```
### 解法三:双指针
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
        if(head==null || head.next==null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode slow = dummy;
        ListNode fast = head;
        while(fast!=null && fast.next!=null) {
            //初始化的时slow指向的是哑结点，所以比较逻辑应该是slow的下一个节点和fast的下一个节点
            if(slow.next.val!=fast.next.val) {
                slow = slow.next;
                fast = fast.next;
            }
            else {
                //如果slow、fast指向的节点值相等，就不断移动fast，直到slow、fast指向的值不相等 
                while(fast!=null && fast.next!=null && slow.next.val==fast.next.val) {
                    fast = fast.next;
                }
                slow.next = fast.next;
                fast = fast.next;
            }
        }
        return dummy.next;
    }
}
```
