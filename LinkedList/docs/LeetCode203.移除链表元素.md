# [LeetCode203.移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)
## 题目描述
删除链表中等于给定值 `val` 的所有节点。

### 示例
```java
输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5
```
## 题解
### 解法一:假节点
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
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyHead=new ListNode(0);
        dummyHead.next=head;
        ListNode cur=dummyHead;
        while(cur.next!=null){
            if(cur.next.val==val){
                cur.next=cur.next.next;
            }else{
                cur=cur.next;
            }
        }
        return dummyHead.next;
    }
}
```
### 复杂度分析

- 时间复杂度：$O(N)$，只遍历了一次。
- 空间复杂度：$O(1)$。
### 解法二：递归
#### 写法一
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
    public ListNode removeElements(ListNode head, int val) {
        while(head!=null&&head.val==val){
            head=head.next;
        }
        if(head==null){
            return null;
        }
        head.next=removeElements(head.next,val);
        return head;
    }
}
```
#### 写法二
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
       if(head==null)
           return null;
        head.next=removeElements(head.next,val);
        if(head.val==val){
            return head.next;
        }else{
            return head;
        }
    }
}
```