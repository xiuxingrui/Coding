# [LeetCode24.两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
## 题目描述
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。
### 示例
```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```
## 题解
### 解法一:递归
1. 从链表的头节点 `head` 开始递归。
2. 每次递归都负责交换一对节点。由 `firstNode` 和 `secondNode` 表示要交换的两个节点。
3. 下一次递归则是传递的是下一对需要交换的节点。若链表中还有节点，则继续递归。
4. 交换了两个节点以后，返回 `secondNode`，因为它是交换后的新头。
5. 在所有节点交换完成以后，我们返回交换后的头，实际上是原始链表的第二个节点。

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
    public ListNode swapPairs(ListNode head) {

        // If the list has no node or has only one node left.
        if ((head == null) || (head.next == null)) {
            return head;
        }

        // Nodes to be swapped
        ListNode firstNode = head;
        ListNode secondNode = head.next;

        // Swapping
        firstNode.next  = swapPairs(secondNode.next);
        secondNode.next = firstNode;

        // Now the head is the second node
        return secondNode;
    }
}
```
#### 复杂度分析

时间复杂度：$O(N)$，其中 $N$ 指的是链表的节点数量。

空间复杂度：$0(N)$，递归过程使用的堆栈空间。

### 解法二:迭代
`A` 指的是交换节点中的前面的节点，`B` 指的是要交换节点中的后面的节点。在完成它们的交换，我们还得用 `prevNode` 记录 `A` 的前驱节点。
1. `firstNode`（即 `A`） 和 `secondNode`（即 `B`） 分别遍历偶数节点和奇数节点，即两步看作一步。
2. 交换两个节点：
```java
 firstNode.next = secondNode.next
 secondNode.next = firstNode
```
3. 还需要更新 `prevNode.next` 指向交换后的头。
```java
prevNode.next = secondNode
```
4. 迭代完成后得到最终的交换结果。

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
    public ListNode swapPairs(ListNode head) {

        // Dummy node acts as the prevNode for the head node
        // of the list and hence stores pointer to the head node.
        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        ListNode prevNode = dummy;

        while ((head != null) && (head.next != null)) {

            // Nodes to be swapped
            ListNode firstNode = head;
            ListNode secondNode = head.next;

            // Swapping
            prevNode.next = secondNode;
            firstNode.next = secondNode.next;
            secondNode.next = firstNode;

            // Reinitializing the head and prevNode for next swap
            prevNode = firstNode;
            head = firstNode.next; // jump
        }

        // Return the new head node.
        return dummy.next;
    }
}
```
#### 复杂度分析

时间复杂度：$O(N)$，其中 $N$ 指的是链表的节点数量。

空间复杂度：$O(1)$。

