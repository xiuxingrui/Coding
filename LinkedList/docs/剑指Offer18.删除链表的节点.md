# [剑指Offer18.删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)
## 题目描述
给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。题目保证链表中节点的值互不相同。

返回删除后的链表的头节点。

### 示例
```
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.

输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.

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
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        ListNode dummyHead=new ListNode(0);
        dummyHead.next=head;
        ListNode cur=dummyHead;
        while(cur!=null){
            while(cur.next!=null&&cur.next.val==val){
                cur.next=cur.next.next;
            }
            cur=cur.next;
        }
        return dummyHead.next;
    }
}
```
或
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
    public ListNode deleteNode(ListNode head, int val) {
        ListNode dummyHead=new ListNode(0);
        ListNode tail=dummyHead;
        while(head!=null){
            if(head.val!=val){
                tail.next=head;
                tail=tail.next;
            }
            head=head.next;
        }
        tail.next=null;
        return dummyHead.next;
    }
}
```
### 复杂度分析
时间复杂度：$O(N)$。`N` 为链表的长度，最坏情况下，要删除的结点位于链表末尾，这时我们需要遍历整个链表。
空间复杂度：$O(1)$。仅使用了额外的 `dummyHead`。


