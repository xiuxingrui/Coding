# [LeetCode25.K个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
## 题目描述
给你一个链表，每 $k$ 个节点一组进行翻转，请你返回翻转后的链表。

$k$ 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 $k$ 的整数倍，那么请将最后剩余的节点保持原有顺序。

### 示例
```
给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5
```
### 说明
1. 你的算法只能使用常数的额外空间。
2. 你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

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
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null) return null;
        // 区间 [a, b) 包含 k 个待反转元素
        ListNode a, b;
        a = b = head;
        for (int i = 0; i < k; i++) {
            // 不足 k 个，不需要反转，base case
            if (b == null) return head;
            b = b.next;
        }
        // 反转前 k 个元素
        ListNode newHead = reverse(a, b);
        // 递归反转后续链表并连接起来
        a.next = reverseKGroup(b, k);
        return newHead;
    }
    /** 反转区间 [a, b) 的元素，注意是左闭右开 */
    ListNode reverse(ListNode a, ListNode b) {
        ListNode pre, cur, nxt;
        pre = null; cur = a; nxt = a;
        // while 终止的条件改一下就行了
        while (cur != b) {
            nxt = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nxt;
        }
        // 返回反转后的头结点
        return pre;
    }
}
```
### 解法二:迭代
1. 链表分区为已翻转部分+待翻转部分+未翻转部分
2. 每次翻转前，要确定翻转链表的范围，这个必须通过 `k` 此循环来确定
3. 需记录翻转链表前驱和后继，方便翻转完成后把已翻转部分和未翻转部分连接起来
4. 初始需要两个变量 `pre` 和 `end`，`pre` 代表待翻转链表的前驱，`end` 代表待翻转链表的末尾
5. 经过`k`次循环，`end` 到达末尾，记录待翻转链表的后继 `next = end.next`
6. 翻转链表，然后将三部分链表连接起来，然后重置 `pre` 和 `end` 指针，然后进入下一次循环
7. 特殊情况，当翻转部分长度不足 `k` 时，在定位 `end` 完成后，`end==null`，已经到达末尾，说明题目已完成，直接返回即可

```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode pre = dummy;
    ListNode end = dummy;

    while (end.next != null) {
        for (int i = 0; i < k && end != null; i++) end = end.next;
        if (end == null) break;
        ListNode start = pre.next;
        ListNode next = end.next;
        end.next = null;
        pre.next = reverse(start);
        start.next = next;
        pre = start;

        end = pre;
    }
    return dummy.next;
}

private ListNode reverse(ListNode head) {
    ListNode pre = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = pre;
        pre = curr;
        curr = next;
    }
    return pre;
}
```


