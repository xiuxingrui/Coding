# [LeetCode2.两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
## 题目描述
给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

### 示例
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```
## 题解
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode p = l1, q = l2, curr = dummyHead;
        int carry = 0;
        while (p != null || q != null||carry>0) {
            int x = (p != null) ? p.val : 0;
            int y = (q != null) ? q.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            if (p != null) p = p.next;
            if (q != null) q = q.next;
        }
        return dummyHead.next;
    }
}
```
原地解法:
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int flag=0;
        ListNode ans=l1;
        while(l1.next!=null&&l2.next!=null){
            l1.val+=l2.val+flag;
            flag=l1.val/10;
            l1.val%=10;
            l1=l1.next;
            l2=l2.next;
        }
        l1.val+=l2.val+flag;
        flag=l1.val/10;
        l1.val%=10;
        if(l1.next==null){
            if(l2.next!=null){
                l1.next=l2.next;
                l1=l1.next;
                while(l1!=null&&l1.next!=null){
                    l1.val+=flag;
                    flag=l1.val/10;
                    l1.val=l1.val%10;
                    l1=l1.next;
                }
                l1.val+=flag;
                flag=l1.val/10;
                l1.val=l1.val%10;
                if(flag==1){
                    l1.next=new ListNode(1);
                }
                return ans;
            }else{
                if(flag==1){
                    l1.next=new ListNode(1);
                }
                return ans;
            }
        }else{
            l1=l1.next;
            while(l1.next!=null){
                l1.val+=flag;
                flag=l1.val/10;
                l1.val=l1.val%10;
                l1=l1.next;
            }
            l1.val+=flag;
            flag=l1.val/10;
            l1.val=l1.val%10;
            if(flag==1){
                l1.next=new ListNode(1);
            }
            return ans;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(max(m,n))$，假设 `m` 和 `n` 分别表示 `l1` 和 `l2` 的长度，上面的算法最多重复 `max(m,n)`次。

- 空间复杂度：$O(max(m,n))$， 新列表的长度最多为 `max(m,n)+1`。


