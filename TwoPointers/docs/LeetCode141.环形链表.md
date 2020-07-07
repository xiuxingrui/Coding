# [LeetCode141.环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
## 题目描述
给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 -1，则在该链表中没有环。
### 示例
```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200707183743.png)

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200707183758.png)

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200707183814.png)

## 题解
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
    public boolean hasCycle(ListNode head) {
        ListNode slow=head,fast=head;
        while(fast!=null&&fast.next!=null){
            slow=slow.next;
            fast=fast.next.next;
            if(slow==fast){
                return true;
            }
        }
        return false;
    }
}
```
#### 复杂度分析
- 时间复杂度:$O(n)$，让我们将 `n` 设为链表中结点的总数。为了分析时间复杂度，我们分别考虑下面两种情况。
  - 链表中不存在环：快指针将会首先到达尾部，其时间取决于列表的长度，也就是 $O(n)$。 
  - 链表中存在环：
  我们将慢指针的移动过程划分为两个阶段：非环部分与环形部分： 
  1. 慢指针在走完非环部分阶段后将进入环形部分：此时，快指针已经进入环中 迭代次数=非环部分长度=`N`
  2. 两个指针都在环形区域中：考虑两个在环形赛道上的运动员 - 快跑者每次移动两步而慢跑者每次只移动一步。其速度的差值为 1，因此需要经过(二者之间距离/速度差值)次循环后，快跑者可以追上慢跑者。这个距离几乎就是 "环形部分长度`K`" 且速度差值为 1，我们得出这样的结论 迭代次数=近似于环形部分长度 `K`".

  因此，在最糟糕的情形下，时间复杂度为 $O(N+K)$，也就是 $O(n)$。

- 空间复杂度：$O(1)$，我们只使用了慢指针和快指针两个结点，所以空间复杂度为 $O(1)$。


​	
 
