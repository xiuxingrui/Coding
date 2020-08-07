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
        Deque<ListNode> s1=new LinkedList<>();
        Deque<ListNode> s2=new LinkedList<>();
        while(l1!=null){
            s1.push(l1);
            l1=l1.next;
        }
        while(l2!=null){
            s2.push(l2);
            l2=l2.next;
        }
        int flag=0;
        ListNode dummyHead=new ListNode(0);
        ListNode cur=dummyHead;
        while(!s1.isEmpty()||!s2.isEmpty()){
            int val1=s1.isEmpty()?0:s1.pop().val;
            int val2=s2.isEmpty()?0:s2.pop().val;
            int sum=flag+val1+val2;
            flag=sum/10;
            ListNode temp=new ListNode(sum%10);
            temp.next=dummyHead.next;
            dummyHead.next=temp;
        }
        if(flag==1){
            ListNode temp=new ListNode(1);
            temp.next=dummyHead.next;
            dummyHead.next=temp;
        }
        return dummyHead.next;
    }
}
```
### 解法二
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
        l1=reverseListNode(l1);
        l2=reverseListNode(l2);
        ListNode dummyHead=new ListNode(0);
        ListNode tail=dummyHead;
        int flag=0;
        while(l1!=null||l2!=null||flag==1){
            int val1=l1==null?0:l1.val;
            int val2=l2==null?0:l2.val;
            int sum=flag+val1+val2;
            flag=sum/10;
            tail.next=new ListNode(sum%10);
            tail=tail.next;
            if(l1!=null){
                l1=l1.next;
            }
            if(l2!=null){
                l2=l2.next;
            }
        }
        return reverseListNode(dummyHead.next);
    }

    public ListNode reverseListNode(ListNode node){
        ListNode pre=null,cur=node;
        while(cur!=null){
            ListNode temp=cur.next;
            cur.next=pre;
            pre=cur;
            cur=temp;
        }
        return pre;
    }
}
```
