# [LeetCode876.链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)
## 题目描述
给定一个带有头结点 `head` 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

### 示例
```
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```
```
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。
```
## 题解
### 解法一
快慢指针：
题目要求：「两个中间结点的时候，返回**第二个**中间结点」，**快指针可以前进的条件是：当前快指针和当前快指针的下一个结点都非空**。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200708115500.png)
如果题目要求「在两个中间结点的时候，返回**第一个**中间结点」，**此时快指针可以前进的条件是：当前快指针的下一个结点和当前快指针的下下一个结点都非空**。
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
    public ListNode middleNode(ListNode head) {
        ListNode slow=head,fast=head;   
        while(fast!=null&&fast.next!=null){
            slow=slow.next;
            fast=fast.next.next;
        }
        return slow;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(N)$，其中 `N` 是给定链表的结点数目。

- 空间复杂度：$O(1)$，只需要常数空间存放 `slow` 和 `fast` 两个指针。

### 解法二
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        int n = 0;
        ListNode cur = head;
        while (cur != null) {
            ++n;
            cur = cur.next;
        }
        int k = 0;
        cur = head;
        while (k < n / 2) {
            ++k;
            cur = cur.next;
        }
        return cur;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(N)$，其中 `N` 是给定链表的结点数目。

- 空间复杂度：$O(1)$，只需要常数空间存放变量和指针。


