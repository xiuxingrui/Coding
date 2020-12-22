# [剑指Offer22.链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
## 题目描述
输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

### 示例
```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```
## 题解
使用双指针可以不用统计链表长度。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201222184722.png)

算法流程：

1. 初始化： 前指针 `former` 、后指针 `latter` ，双指针都指向头节点 `head`​ 。
2. 构建双指针距离： 前指针 `former` 先向前走 `k` 步（结束后，双指针 `former` 和 `latter` 间相距 `k` 步）。
3. 双指针共同移动： 循环中，双指针 `former` 和 `latter` 每轮都向前走一步，直至 `former` 走过链表 尾节点 时跳出（跳出后， `latter` 与尾节点距离为 `k−1`，即 `latter` 指向倒数第 `k` 个节点）。
4. 返回值： 返回 `latter` 即可。

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
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head==null){
            return null;
        }
        ListNode fast=head;
        for(int i=0;i<k;i++){
            if(fast==null){
                return null;
            }
            fast=fast.next;
        }
        ListNode slow=head;
        while(fast!=null){
            fast=fast.next;
            slow=slow.next;
        }
        return slow;
    }
}
```
### 复杂度分析
- 时间复杂度 $O(N)$ ： `N` 为链表长度；总体看， `former` 走了`N` 步， `latter` 走了 `(N−k)` 步。
- 空间复杂度 $O(1)$ ： 双指针 `former` , `latter` 使用常数大小的额外空间。



