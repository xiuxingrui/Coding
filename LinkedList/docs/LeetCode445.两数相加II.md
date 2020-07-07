# [LeetCode445.两数相加II](https://leetcode-cn.com/problems/add-two-numbers-ii/)
## 题目描述
给你两个 **非空** 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
### 示例
```
输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 8 -> 0 -> 7
```
## 题解
### 解法一
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) { 
        Deque<Integer> stack1 = new LinkedList<>();
        Deque<Integer> stack2 = new LinkedList<>();
        while (l1 != null) {
            stack1.offerLast(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            stack2.offerLast(l2.val);
            l2 = l2.next;
        }
        
        int flag = 0;
        ListNode head = null;
        while (!stack1.isEmpty() || !stack2.isEmpty()) {
            int val1= stack1.isEmpty()? 0: stack1.pollLast();
            int val2= stack2.isEmpty()? 0: stack2.pollLast();
            int sum = val1+val2+flag;
            ListNode node = new ListNode(sum % 10);
            node.next = head;
            head = node;
            flag = sum / 10;
        }
        if(flag==1){
            ListNode node=new ListNode(1);
            node.next=head;
            head=node;
        }
        return head;
    }
}
```
### 解法二
反转链表后按[LeetCode2.两数相加](LeetCode2.两数相加.md)去做。