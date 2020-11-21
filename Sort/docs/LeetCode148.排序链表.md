# [LeetCode148.排序链表](https://leetcode-cn.com/problems/sort-list/)
## 题目描述
在 $O(nlogn)$ 时间复杂度和常数级空间复杂度下，对链表进行排序。
### 示例
```
输入: 4->2->1->3
输出: 1->2->3->4
```
```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```
## 题解
### 解法一:归并排序
题目要求时间空间复杂度分别为$O(nlogn)$和$O(1)$，根据时间复杂度我们自然想到二分法，从而联想到归并排序；

对数组做归并排序的空间复杂度为 $O(n)$，分别由新开辟数组$O(n)$和递归函数调用$O(logn)$组成，而根据链表特性：

- 数组额外空间：链表可以通过修改引用来更改节点顺序，无需像数组一样开辟额外空间；
- 递归额外空间：递归调用函数将带来$O(logn)$的空间复杂度，因此若希望达到$O(1)$空间复杂度，则不能使用递归。

通过递归实现链表归并排序，有以下两个环节：

分割 `cut` 环节： 找到当前链表中点，并从中点将链表断开（以便在下次递归 `cut` 时，链表片段拥有正确边界）；

- 我们使用 `fast`,`slow` 快慢双指针法，奇数个节点找到中点，偶数个节点找到中心左边的节点。
- 找到中点 `slow` 后，执行 `slow.next = null` 将链表切断。
- 递归分割时，输入当前链表左端点 `head` 和中心节点 `slow` 的下一个节点 `temp`(因为链表是从 `slow` 切断的)。
- `cut` 递归终止条件： 当`head.next == null`时，说明只有一个节点了，直接返回此节点。

- 合并 `merge` 环节： 将两个排序链表合并，转化为一个排序链表。
- 双指针法合并，建立辅助`ListNode dummyHead` 作为头部。
- 两指针分别指向两链表头部，比较两指针处节点值大小，由小到大加入合并链表头部，指针交替前进，直至添加完两个链表。
- 返回辅助`ListNode dummyHead` 作为头部的下个节点 `dummyHead.next`。
- 时间复杂度 $O(l + r)$，`l`, `r` 分别代表两个链表长度。

当题目输入的 `head == null` 时，直接返回`null`。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821184039.png)s
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
    public ListNode sortList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        ListNode midNode=getMid(head);
        ListNode temp=midNode.next;
        midNode.next=null;
        ListNode l1=sortList(head);
        ListNode l2=sortList(temp);
        return merge(l1,l2);
    }
    public ListNode getMid(ListNode head){
        ListNode slow=head,fast=head;
        while(fast.next!=null&&fast.next.next!=null){
            slow=slow.next;
            fast=fast.next.next;
        }
        return slow;
    }
    public ListNode merge(ListNode l1,ListNode l2){
        ListNode dummyHead=new ListNode(0);
        ListNode cur=dummyHead;
        while(l1!=null&&l2!=null){
            if(l1.val<=l2.val){
                cur.next=l1;
                l1=l1.next;
            }else{
                cur.next=l2;
                l2=l2.next;
            }
            cur=cur.next;
        }
        cur.next=l1==null?l2:l1;
        return dummyHead.next;
    }
}
```
### 解法二:快速排序
`leetcode` 86题，分隔链表
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200821235722.png)
指针交换写法:
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
    public ListNode sortList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        int pivot=head.val;
        ListNode dummy1=new ListNode(0);
        ListNode p1=dummy1;
        ListNode dummy2=new ListNode(0);
        ListNode p2=dummy2;
        ListNode cur=head;
        while(cur!=null){
            if(cur.val>=pivot){//因为head要在右边第一个，所以判断条件这么写
                p2.next=cur;
                p2=p2.next;
            }else{
                p1.next=cur;
                p1=p1.next;
            }
            cur=cur.next;
        }
        p1.next=null;
        p2.next=null;
        ListNode left=sortList(dummy1.next);
        ListNode right=sortList(head.next);
        if(left==null){
            head.next=right;
            return head;
        }else{
            cur=left;
            while(cur.next!=null){
                cur=cur.next;
            }
            cur.next=head;
            head.next=right;
            return left;
        } 
    }
}
```
值交换写法:
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
    public ListNode sortList(ListNode head) {
        quickSort(head, null);
        return head;
    }
    
    public void quickSort(ListNode head, ListNode tail){
        if(head == tail || head.next == tail) return;
        int pivot = head.val;
        ListNode left = head, cur = head.next;
        
        while(cur != tail){
            if(cur.val < pivot){
                left = left.next;
                swap(left, cur);
            }
            cur = cur.next;
        }
        swap(head, left);
        quickSort(head, left);
        quickSort(left.next, tail);
    }
    public void swap(ListNode l1,ListNode l2){
        int temp=l1.val;
        l1.val=l2.val;
        l2.val=temp;
    }
}
```
### 解法三:插入排序
见`LeetCode147.对链表进行插入排序`：

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
### 解法四:
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
    public ListNode sortList(ListNode head) {
        if (head==null|| head.next==null){
		    return head;
        }
        ListNode dummyhead = new ListNode(-1);//伪头指针
        dummyhead.next = head;
        ListNode prev = head;
        ListNode cur = head.next;
        while (cur!=null){
            if (cur.val < prev.val){
                ListNode temp = dummyhead;//！！temp要等于dummyhead，这样才可以比较第一个元素
                while (temp.next.val < cur.val){//！！！这里是temp->next：因为要修改前面的temp的指向
                    temp = temp.next;//指针后移
                }
                prev.next = cur.next;
                cur.next = temp.next;
                temp.next = cur;
                cur = prev.next;//此时不用改变prev指向！因为prev没有变，只是待排序元素变了位置。
            }else{
                prev = prev.next;
                cur = cur.next;
            }
        }
        return dummyhead.next;//!!!不能返回head！！因为后面会改变head所指向内存的位置！
    }
}
```
