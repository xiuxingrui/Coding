# [LeetCode328.奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)
## 题目描述
给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 $O(1)$，时间复杂度应为 $O(nodes)$，`nodes` 为节点总数。

说明:

- 应当保持奇数节点和偶数节点的相对顺序。
- 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。
### 示例
```
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
```
```
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```
## 题解
### 解法一
如果链表为空，则直接返回链表。

对于原始链表，每个节点都是奇数节点或偶数节点。头节点是奇数节点，头节点的后一个节点是偶数节点，相邻节点的奇偶性不同。因此可以将奇数节点和偶数节点分离成奇数链表和偶数链表，然后将偶数链表连接在奇数链表之后，合并后的链表即为结果链表。

原始链表的头节点 `head` 也是奇数链表的头节点以及结果链表的头节点，`head` 的后一个节点是偶数链表的头节点。令 `evenHead = head.next`，则 `evenHead` 是偶数链表的头节点。

维护两个指针 `odd` 和 `even` 分别指向奇数节点和偶数节点，初始时 `odd = head`，`even = evenHead`。通过迭代的方式将奇数节点和偶数节点分离成两个链表，每一步首先更新奇数节点，然后更新偶数节点。

更新奇数节点时，奇数节点的后一个节点需要指向偶数节点的后一个节点，因此令`odd.next = even.next`，然后令 `odd = odd.next`，此时 `odd` 变成 `even` 的后一个节点。

更新偶数节点时，偶数节点的后一个节点需要指向奇数节点的后一个节点，因此令 `even.next = odd.next`，然后令 `even = even.next`，此时 `even` 变成 `odd`的后一个节点。

在上述操作之后，即完成了对一个奇数节点和一个偶数节点的分离。重复上述操作，直到全部节点分离完毕。全部节点分离完毕的条件是 `even` 为空节点或者 `even.next` 为空节点，此时 `odd` 指向最后一个奇数节点（即奇数链表的最后一个节点）。

最后令 `odd.next = evenHead`，将偶数链表连接在奇数链表之后，即完成了奇数链表和偶数链表的合并，结果链表的头节点仍然是 `head`。

偶链表个数要么和奇链表个数相同要么少1，所以遍历时以偶链表为准，相同时，`even.next=null`,`odd.next=null`,少1时，`even=null`,`odd.next=null`,所以`odd`永远指向奇链表最后一个节点。
```java
public class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;
        ListNode odd = head, even = head.next, evenHead = even;
        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```
或者：（暴力判断）
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
    public ListNode oddEvenList(ListNode head) {
        if(head==null){
            return null;
        }
        ListNode odd=head,evenHead=head.next,even=head.next;
        while(even!=null&&even.next!=null&&odd!=null&&odd.next!=null){
            odd.next=odd.next.next;
            even.next=even.next.next;
            odd=odd.next;
            even=even.next;
        }
        odd.next=evenHead;
        return head;
    }
}
```
#### 复杂度分析
- 时间复杂度： $O(n)$ 。总共有 `n` 个节点，我们每个遍历一次。
- 空间复杂度： $O(1)$ 。
### 解法二
自己的解法:

- 分别定义奇偶链表；
- 遍历原链表，将当前结点交替插入到奇链表和偶链表（尾插法）；
- 将偶链表拼接在奇链表后面。
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
    public ListNode oddEvenList(ListNode head) {
        ListNode dummyHeadOdd=new ListNode(1);
        ListNode endOdd=dummyHeadOdd;
        ListNode dummyHeadEven=new ListNode(2);
        ListNode endEven=dummyHeadEven;
        int flag=1;
        while(head!=null){
            if(flag%2==1){
                endOdd.next=head;
                endOdd=head;
            }else{
                endEven.next=head;
                endEven=head;
            }
            flag++;
            head=head.next;
        }
        endEven.next=null;
        endOdd.next=dummyHeadEven.next;
        return dummyHeadOdd.next;
    }
}
```


