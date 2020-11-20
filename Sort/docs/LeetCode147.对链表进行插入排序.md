# [LeetCode147.对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)
## 题目描述
对链表进行插入排序。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200717153724.gif)

插入排序的动画演示如上。从第一个元素开始，该链表可以被认为已经部分排序（用黑色表示）。
每次迭代时，从输入数据中移除一个元素（用红色表示），并原地将其插入到已排好序的链表中。

插入排序算法：
1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
3. 重复直到所有输入数据插入完为止。

### 示例
```java
输入: 4->2->1->3
输出: 1->2->3->4

输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

## 题解
插入排序的基本思想是，维护一个有序序列，初始时有序序列只有一个元素，每次将一个新的元素插入到有序序列中，将有序序列的长度增加 1，直到全部元素都加入到有序序列中。

如果是数组的插入排序，则数组的前面部分是有序序列，每次找到有序序列后面的第一个元素（待插入元素）的插入位置，将有序序列中的插入位置后面的元素都往后移动一位，然后将待插入元素置于插入位置。

对于链表而言，插入元素时只要更新相邻节点的指针即可，不需要像数组一样将插入位置后面的元素往后移动，因此插入操作的时间复杂度是 $O(1)$，但是找到插入位置需要遍历链表中的节点，时间复杂度是 $O(n)$，因此链表插入排序的总时间复杂度仍然是 $O(n^2)$，其中 `n` 是链表的长度。

对于单向链表而言，只有指向后一个节点的指针，因此需要从链表的头节点开始往后遍历链表中的节点，寻找插入位置。

对链表进行插入排序的具体过程如下。

1. 首先判断给定的链表是否为空，若为空，则不需要进行排序，直接返回。
2. 创建哑节点 `dummyHead`，令 `dummyHead.next = head`。引入哑节点是为了便于在 `head` 节点之前插入节点。
3. 维护 `tail` 为链表的已排序部分的最后一个节点，初始时 `tail = head`。
4. 维护 `cur` 为待插入的元素，初始时 `cur = head.next`。
5. 比较 `tail` 和 `cur` 的节点值。
   1. 若 `tail.val <= cur.val`，说明 `cur` 应该位于 `tail` 之后，将 `tail` 后移一位，`cur` 变成新的 `tail`。
   2. 否则，从链表的头节点开始往后遍历链表中的节点，寻找插入 `cur` 的位置。令 `pre` 为插入 `cur` 的位置的前一个节点，进行如下操作，完成对 `cur` 的插入：
   ```java
   lastSorted.next = curr.next
   curr.next = prev.next 
   prev.next = curr
   ```
6. 令 `cur = tail.next`，此时 `cur` 为下一个待插入的元素。
7. 重复第 5 步和第 6 步，直到 `cur` 变成空，排序结束。
8. 返回 `dummyHead.next`，为排序后的链表的头节点。

利用尾结点提速(3ms):
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
    public ListNode insertionSortList(ListNode head) {
        if(head==null){
            return null;
        }
        ListNode dummyHead=new ListNode(0);
        dummyHead.next=head;
        ListNode tail=head;
        ListNode cur=head.next;
        while(cur!=null){
            if(cur.val>=tail.val){
                //tail.next=cur;不写也行
                tail=tail.next;
                cur=cur.next;
            }else{
                ListNode pre=dummyHead;
                while(pre.next.val<cur.val){
                    pre=pre.next;
                }
                tail.next=cur.next;
                cur.next=pre.next;
                pre.next=cur;
                cur=tail.next;
            }
        }
        return dummyHead.next;
    }
}
```
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
    public ListNode insertionSortList(ListNode head) {
        if(head==null){
            return null;
        }
        ListNode dummy = new ListNode(0);
        dummy.next=head;
        ListNode pre = dummy;
        ListNode tail = head;
        ListNode cur = head.next;
        while (cur != null) {
            if (tail.val <=cur.val) {
                tail.next = cur;
                tail = cur;
                cur = cur.next;
            } else {
                pre = dummy;
                ListNode tmp = cur.next;
                tail.next = tmp;
                //while (pre.next.val < cur.val) pre = pre.next;无需判断是否为空也行
                while (pre.next != null && pre.next.val < cur.val) pre = pre.next;
                cur.next = pre.next;
                pre.next = cur;
                cur = tmp;
            }
        }
        return dummy.next;
    }
}
```
自己的解法：(28ms)
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
    public ListNode insertionSortList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        ListNode dummyHead=new ListNode(0);
        dummyHead.next=head;
        ListNode cur=head.next;
        head.next=null;
        while(cur!=null){
            ListNode temp=cur.next;
            ListNode pre=dummyHead;
            while(pre.next!=null&&pre.next.val<cur.val){
                pre=pre.next;
            }
            cur.next=pre.next;
            pre.next=cur;
            cur=temp;
        }
        return dummyHead.next;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n^2)$，其中 `n` 是链表的长度。
- 空间复杂度：$O(1)$。

