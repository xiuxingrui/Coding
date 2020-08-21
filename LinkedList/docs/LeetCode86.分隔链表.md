# [LeetCode86.分隔链表](https://leetcode-cn.com/problems/partition-list/)
## 题目描述
给定一个链表和一个特定值 `x`，对链表进行分隔，使得所有小于 `x` 的节点都在大于或等于 `x` 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

### 示例
```
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
```
## 题解
我们可以用两个指针来追踪上述的两个链表。两个指针可以用于分别创建两个链表，然后将这两个链表连接即可获得所需的链表。

1. 初始化两个指针 `before` 和 `after`。在实现中，我们将两个指针初始化为哑 `ListNode`。这有助于减少条件判断。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821195118.png)

2. 利用`head`指针遍历原链表。
3. 若`head` 指针指向的元素值 小于 `x`，该节点应当是 `before` 链表的一部分。因此我们将其移到 `before` 中。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821195159.png)
4. 否则，该节点应当是`after` 链表的一部分。因此我们将其移到 `after` 中。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821195235.png)
5. 遍历完原有链表的全部元素之后，我们得到了两个链表 `before` 和 `after`。原有链表的元素或者在`before` 中或者在 `after` 中，这取决于它们的值。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821195303.png)
由于我们从左到右遍历了原有链表，故两个链表中元素的相对顺序不会发生变化。另外值得注意的是，在图中我们完好地保留了原有链表。事实上，在算法实现中，我们将节点从原有链表中移除，并将它们添加到别的链表中。我们没有使用任何额外的空间，只是将原有的链表元素进行移动。
6. 现在，可以将 `before` 和 `after` 连接，组成所求的链表。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821195354.png)

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
    public ListNode partition(ListNode head, int x) {
        ListNode dummy1 = new ListNode(-1);
        ListNode dummy2 = new ListNode(-1);
        ListNode p1 = dummy1;
        ListNode p2 = dummy2;
        while (head != null) {
            if (head.val < x) {
                p1.next = head;
                p1 = p1.next;
            } else {
                p2.next = head;
                p2 = p2.next;
            }
            head = head.next;
        }
        p1.next = dummy2.next;
        p2.next = null;
        return dummy1.next; 
    }
}
```
### 复杂度分析
- 时间复杂度: $O(N)$，其中NNN是原链表的长度，我们对该链表进行了遍历。
- 空间复杂度: $O(1)$，我们没有申请任何新空间。值得注意的是，我们只移动了原有的结点，因此没有使用任何额外空间。

