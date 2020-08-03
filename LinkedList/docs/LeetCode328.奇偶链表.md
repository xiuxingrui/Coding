# [LeetCode328.奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)
## 题目描述
给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 $O(1)$，时间复杂度应为 $O(nodes)$，`nodes` 为节点总数。

说明:

- 应当保持奇数节点和偶数节点的相对顺序。
- 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。
### 示例
```
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
```
```
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```
## 题解
### 解法一
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
    public ListNode oddEvenList(ListNode head) {
        if(head==null){
            return null;
        }
        ListNode odd=head,evenHead=head.next,even=head.next;
        while(odd.next!=null&&even.next!=null){
            odd.next=odd.next.next;
            even.next=even.next.next;
            odd=odd.next;
            even=even.next;
        }
        odd.next=evenHead;
        return head;
    }
}
```
或者:
```java
public class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;
        ListNode odd = head, even = head.next, evenHead = even;
        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```
#### 复杂度分析
- 时间复杂度： $O(n)$ 。总共有 `n` 个节点，我们每个遍历一次。
- 空间复杂度： $O(1)$ 。
### 解法二
自己的解法
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
    public ListNode oddEvenList(ListNode head) {
        ListNode oddHead=new ListNode(0);
        ListNode evenHead=new ListNode(0);
        oddHead.next=null;
        evenHead.next=null;
        ListNode oddTail=oddHead,evenTail=evenHead;
        ListNode cur=head;
        int cnt=1;
        while(cur!=null){
            if(cnt%2==1){
                oddTail.next=cur;
                oddTail=cur;
            }else{
                evenTail.next=cur;
                evenTail=cur;
            }
            cnt++;
            cur=cur.next;
        }
        if(evenTail!=null){
            evenTail.next=null;
        }
        oddTail.next=evenHead.next;
        return oddHead.next;
    }
}
```


