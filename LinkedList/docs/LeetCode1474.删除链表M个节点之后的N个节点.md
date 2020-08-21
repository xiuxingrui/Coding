# [LeetCode1474.删除链表M个节点之后的N个节点](https://leetcode-cn.com/problems/delete-n-nodes-after-m-nodes-of-a-linked-list/)
## 题目描述
给定链表 `head` 和两个整数 `m` 和 `n`. 遍历该链表并按照如下方式删除节点:
- 开始时以头节点作为当前节点.
- 保留以当前节点开始的前 `m` 个节点.
- 删除接下来的 `n` 个节点.
- 重复步骤 2 和 3, 直到到达链表结尾.

在删除了指定结点之后, 返回修改过后的链表的头节点.

- `1 <= 链表结点数 <= 10^4`
- `[1 <= 链表的每一个结点值 <=10^6]`
- `1 <= m,n <= 1000`
### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821170533.png)
```
输入: head = [1,2,3,4,5,6,7,8,9,10,11,12,13], m = 2, n = 3
输出: [1,2,6,7,11,12]
解析: 保留前(m = 2)个结点,  也就是以黑色节点表示的从链表头结点开始的结点(1 ->2).
删除接下来的(n = 3)个结点(3 -> 4 -> 5), 在图中以红色结点表示.
继续相同的操作, 直到链表的末尾.
返回删除结点之后的链表的头结点.
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821170552.png)
```
输入: head = [1,2,3,4,5,6,7,8,9,10,11], m = 1, n = 3
输出: [1,5,9]
解析: 返回删除结点之后的链表的头结点.
```
```
输入: head = [1,2,3,4,5,6,7,8,9,10,11], m = 3, n = 1
输出: [1,2,3,5,6,7,9,10,11]
```
```
输入: head = [9,3,7,7,9,10,8,2], m = 1, n = 2
输出: [9,7,8]
```
## 题解
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteNodes(ListNode head, int m, int n) {
        ListNode dummyHead=new ListNode(0);
        dummyHead.next=head;
        ListNode cur=head;
        while(cur!=null){
            int cntM=0;
            while(cur!=null&&cntM<m-1){
                cur=cur.next;
                cntM++;
            }
            ListNode temp=cur;
            int cntN=0;
            while(cur!=null&&cntN<n+1){
                cur=cur.next;
                cntN++;
            }
            if(temp!=null){
                temp.next=cur;
            }
        }
        return dummyHead.next;
    }
}
```
