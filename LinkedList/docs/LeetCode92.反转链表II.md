# [LeetCode92.反转链表II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
## 题目描述
反转从位置 `m` 到 `n` 的链表。请使用一趟扫描完成反转。

`1 ≤ m ≤ n ≤ 链表长度。`

### 示例
```
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```
## 题解
### 解法一
迭代：

版本1：
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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode res=new ListNode(0);
        res.next=head;
        ListNode node=res;
        //找到需要反转的那一段的上一个节点。
        for(int i=1;i<m;i++){
            node=node.next;
        }
        //node.next就是需要反转的这段的起点。
        ListNode pre=null,cur=node.next;
        for(int i=m;i<=n;i++){
            ListNode temp=cur.next;
            cur.next=pre;
            pre=cur;
            cur=temp;
        }
        //将反转的起点的next指向cur。
        node.next.next=cur;
        //需要反转的那一段的上一个节点的next节点指向反转后链表的头结点
        node.next=pre;
        return res.next;
    }
}
```
版本2：
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {

        // Empty list
        if (head == null) {
            return null;
        }

        // Move the two pointers until they reach the proper starting point
        // in the list.
        ListNode cur = head, prev = null;
        while (m > 1) {
            prev = cur;
            cur = cur.next;
            m--;
            n--;
        }

        // The two pointers that will fix the final connections.
        ListNode con = prev, tail = cur;

        // Iteratively reverse the nodes until n becomes 0.
        ListNode third = null;
        while (n > 0) {
            third = cur.next;
            cur.next = prev;
            prev = cur;
            cur = third;
            n--;
        }

        // Adjust the final connections as explained in the algorithm
        if (con != null) {
            con.next = prev;
        } else {
            head = prev;
        }

        tail.next = cur;
        return head;
    }
}
```
#### 复杂度分析
- 时间复杂度: $O(N)$。
- 空间复杂度: $O(1)$。我们仅仅在原有链表的基础上调整了一些指针，只使用了 $O(1)$ 的额外存储空间来获得结果。
### 解法二
递归：
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
    ListNode successor = null; // 后驱节点
    public ListNode reverseBetween(ListNode head, int m, int n) {
    // base case
    if (m == 1) {
        return reverseN(head, n);
    }
    // 前进到反转的起点触发 base case
    head.next = reverseBetween(head.next, m - 1, n - 1);
    return head;
    }
    public ListNode reverseN(ListNode head, int n) {
    if (n == 1) { 
        // 记录第 n + 1 个节点
        successor = head.next;
        return head;
    }
    // 以 head.next 为起点，需要反转前 n - 1 个节点
    ListNode last = reverseN(head.next, n - 1);

    head.next.next = head;
    // 让反转之后的 head 节点和后面的节点连起来
    head.next = successor;
    return last;
    }
}
```
