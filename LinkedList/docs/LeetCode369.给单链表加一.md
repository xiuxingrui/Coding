# [LeetCode369.给单链表加一](https://leetcode-cn.com/problems/plus-one-linked-list/)
## 题目描述
用一个 **非空** 单链表来表示一个非负整数，然后将这个整数加一。

你可以假设这个整数除了 0 本身，没有任何前导的 0。

这个整数的各个数位按照 **高位在链表头部、低位在链表尾部** 的顺序排列。

### 示例
```
输入: [1,2,3]
输出: [1,2,4]
```
## 题解
### 解法一
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
    public ListNode plusOne(ListNode head) {
        if (head == null) {
          return head;
        }
        int carry = helper(head);
        if (carry == 1) {
          ListNode res = new ListNode(1);
          res.next = head;
          return res;
        }
        return head;
    }
    public int helper(ListNode node) {
        if (node == null) {
          return 1;
        }
        int carry = helper(node.next);
        int sum = node.val + carry;
        node.val = sum % 10;
        return sum / 10;
      }
}
```
### 解法二
- 初始化哨兵节点 `ListNode(0)`，同时将它设为新的头节点：`sentinel.next = head`。
- 找到最靠右的数值不为 9 的节点。
- 将该节点的数值加 1。
- 将该节点之后所有节点数值改为 0。
- 如果哨兵节点的数值为 1，直接返回哨兵节点，否则返回原始头节点 `sentinel.next`。
```java
class Solution {
  public ListNode plusOne(ListNode head) {
    // sentinel head
    ListNode sentinel = new ListNode(0);
    sentinel.next = head;
    ListNode notNine = sentinel;

    // find the rightmost not-nine digit
    while (head != null) {
      if (head.val != 9) notNine = head;
      head = head.next;
    }
    
    // increase this rightmost not-nine digit by 1
    notNine.val++;
    notNine = notNine.next;
    
    // set all the following nines to zeros
    while (notNine != null) {
      notNine.val = 0;
      notNine = notNine.next;
    }
    
    return sentinel.val != 0 ? sentinel : sentinel.next;
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，最多只需遍历两遍链表。
- 空间复杂度：$O(1)$。

