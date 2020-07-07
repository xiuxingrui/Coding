# [LeetCode142.环形链表II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
## 题目描述
给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 -1，则在该链表中没有环。

**说明**：不允许修改给定的链表。
### 示例
```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200707190300.png)

```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200707190320.png)

```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200707190334.png)

### 题解
当`fast == slow`时， 两指针在环中 第一次相遇 。下面分析此时`fast` 与 `slow`走过的 步数关系 ：

- 设链表共有 `a+b` 个节点，其中 链表头部到链表入口 有 `a` 个节点（不计链表入口节点）， 链表环 有 `b` 个节点（这里需要注意，`a` 和 `b` 是未知数，例如图解上链表 `a=4`, `b=5`）；设两指针分别走了 `f`，`s` 步，则有：
  1. `fast` 走的步数是`slow`步数的 2 倍，即 $f=2s$；(解析： `fast` 每轮走 2 步)
  2. `fast` 比 `slow`多走了 `n` 个环的长度，即 $f=s+nb$；(解析： 双指针都走过 `a` 步，然后在环内绕圈直到重合，重合时 `fast` 比 `slow` 多走 环的长度整数倍)；
- 以上两式相减得：$f=2nb$，$s=nb$，即`fast`和`slow` 指针分别走了 `2n`，`n` 个 环的周长(注意： `n` 是未知数，不同链表的情况不同)。
  
- 如果让指针从链表头部一直向前走并统计步数`k`，那么所有 走到链表入口节点时的步数 是：`k=a+nb`(先走 `a` 步到入口节点，之后每绕 1 圈环(`b` 步)都会再次到入口节点)。
- 而目前，`slow` 指针走过的步数为 `nb` 步。因此，我们只要想办法让 `slow` 再走 `a` 步停下来，就可以到环的入口。

但是我们不知道 `a` 的值，该怎么办？依然是使用双指针法。我们构建一个指针，此指针需要有以下性质：此指针和`slow` 一起向前走 `a`步后，两者在入口节点重合。那么从哪里走到入口节点需要 `a` 步？答案是链表头部`head`。

- `slow`指针 位置不变 ，将`fast`指针重新 指向链表头部节点 ；`slow`和`fast`同时每轮向前走 1 步；此时 `f=0`，`s=nb`；

- 当 `fast` 指针走到`f=a` 步时，`slow` 指针走到步`s=a+nb`，此时 两指针重合，并同时指向链表环入口 。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        ListNode meet = null;
        while (fast != null&&fast.next!=null) {
            slow = slow.next;
            fast = fast.next.next;
            //到达相遇点
            if (fast == slow) {
                meet = fast;
                while (head != meet) {
                    head = head.next;
                    meet = meet.next;
                }
                return head;
            }
        }
        return null;
    }
}
```
#### 复杂度分析
- 时间复杂度:$O(N)$：第二次相遇中，慢指针须走步数 $a<a+b$；第一次相遇中，慢指针须走步数 $a+b−x<a+b$，其中 `x` 为双指针重合点与环入口距离；因此总体为线性复杂度；
- 空间复杂度: $O(1)$：双指针使用常数大小的额外空间。


