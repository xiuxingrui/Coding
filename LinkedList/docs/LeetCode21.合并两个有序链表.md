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
            if(l1.val <= l2.val) {
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
#### 复杂度分析

时间复杂度：$O(n+m)$ ，其中 $n$ 和 $m$ 分别为两个链表的长度。因为每次循环迭代中，$l1$ 和 $l2$ 只有一个元素会被放进合并链表中， 因此 `while` 循环的次数不会超过两个链表的长度之和。所有其他操作的时间复杂度都是常数级别的，因此总的时间复杂度为 $O(n+m)$。

空间复杂度：$O(1)$ 。我们只需要常数的空间存放若干变量。

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
#### 复杂度分析

时间复杂度：$O(n+m)$，其中 $n$ 和 $m$ 分别为两个链表的长度。因为每次调用递归都会去掉 $l1$ 或者 $l2$ 的头节点（直到至少有一个链表为空），函数 `mergeTwoList` 至多只会递归调用每个节点一次。因此，时间复杂度取决于合并后的链表长度，即 $O(n+m)$。

空间复杂度：$O(n+m)$，其中 $n$ 和 $m$ 分别为两个链表的长度。递归调用 `mergeTwoLists` 函数时需要消耗栈空间，栈空间的大小取决于递归调用的深度。结束递归调用时 `mergeTwoLists` 函数最多调用 $n+m$ 次，因此空间复杂度为 $O(n+m)$。
