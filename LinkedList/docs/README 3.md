# [LeetCode234.回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)
## 题目描述
请判断一个链表是否为回文链表。
### 示例
```
输入: 1->2
输出: false

输入: 1->2->2->1
输出: true
```
### 进阶
你能否用 $O(n)$ 时间复杂度和 $O(1)$ 空间复杂度解决此题？
## 题解
### 解法一:快慢指针
避免使用 $O(n)$ 额外空间的方法就是改变输入。

我们可以将链表的后半部分反转（修改链表结构），然后将前半部分和后半部分进行比较。比较完成后我们应该将链表恢复原样。

我们可以分为以下几个步骤：

1. 找到前半部分链表的尾节点。
2. 反转后半部分链表。
3. 判断是否为回文。
4. 恢复链表。
5. 返回结果
版本1：
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
    public boolean isPalindrome(ListNode head) {
        if(head==null){
            return true;
        }
        ListNode slow=head,fast=head;
        while(fast!=null&&fast.next!=null){
            slow=slow.next;
            fast=fast.next.next;
        }
        if(fast!=null){
            slow=slow.next;
        }
        ListNode temp=reverseListNode(slow);
        while(temp!=null){
            if(head.val!=temp.val){
                return false;
            }
            temp=temp.next;
            head=head.next;
        }
        return true;
    }
    public ListNode reverseListNode(ListNode head){
        ListNode pre=null,cur=head;
        while(cur!=null){
            ListNode temp=cur.next;
            cur.next=pre;
            pre=cur;
            cur=temp;
        }
        return pre;
    }
}
```
版本2：
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
    public boolean isPalindrome(ListNode head) {
        if(head==null){
            return true;
        }
        ListNode slow=head,fast=head,pre=null,cur;
        while(fast!=null&&fast.next!=null){
            cur=slow;
            slow=slow.next;
            fast=fast.next.next;

            cur.next=pre;
            pre=cur;
        }
        if(fast!=null){
            slow=slow.next;
        }
        while(slow!=null){
            if(slow.val!=pre.val){
                return false;
            }
            slow=slow.next;
            pre=pre.next;
        }
        return true;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(n)$，其中 `n` 指的是链表的大小。
- 空间复杂度：$O(1)$，我们是一个接着一个的改变指针，我们在堆栈上的堆栈帧不超过 $O(1)$。
该方法的缺点是，在并发环境下，函数运行时需要锁定其他线程或进程对链表的访问，因为在函数执执行过程中链表暂时断开。

### 解法二:将值复制到数组中后用双指针法
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> vals = new ArrayList<>();

        // Convert LinkedList into ArrayList.
        ListNode currentNode = head;
        while (currentNode != null) {
            vals.add(currentNode.val);
            currentNode = currentNode.next;
        }

        // Use two-pointer technique to check for palindrome.
        int front = 0;
        int back = vals.size() - 1;
        while (front < back) {
            // Note that we must use ! .equals instead of !=
            // because we are comparing Integer, not int.
            if (!vals.get(front).equals(vals.get(back))) {
                return false;
            }
            front++;
            back--;
        }
        return true;
    }
}
```
### 复杂度分析
- 时间复杂度：$(n)$，其中 `n` 指的是链表的元素个数。
  - 第一步： 遍历链表并将值复制到数组中，$O(n)$。
  - 第二步：双指针判断是否为回文，执行了 $O(n/2)$ 次的判断，即 $O(n)$。
  - 总的时间复杂度：$O(2n)=O(n)$。
- 空间复杂度：$O(n)$，其中 `n` 指的是链表的元素个数，我们使用了一个数组列表存放链表的元素值。

### 解法三:递归
```java
class Solution {

    private ListNode frontPointer;

    private boolean recursivelyCheck(ListNode currentNode) {
        if (currentNode != null) {
            if (!recursivelyCheck(currentNode.next)) return false;
            if (currentNode.val != frontPointer.val) return false;
            frontPointer = frontPointer.next;
        }
        return true;
    }

    public boolean isPalindrome(ListNode head) {
        frontPointer = head;
        return recursivelyCheck(head);
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(n)$，其中 `n` 指的是链表的大小。
- 空间复杂度：$O(n)$，其中 `n` 指的是链表的大小。我们要理解计算机如何运行递归函数，在一个函数中调用一个函数时，计算机需要在进入被调用函数之前跟踪它在当前函数中的位置（以及任何局部变量的值），通过运行时存放在堆栈中来实现（堆栈帧）。在堆栈中存放好了数据后就可以进入被调用的函数。在完成被调用函数之后，他会弹出堆栈顶部元素，以恢复在进行函数调用之前所在的函数。在进行回文检查之前，递归函数将在堆栈中创建 `n` 个堆栈帧，计算机会逐个弹出进行处理。所以在使用递归时要考虑堆栈的使用情况。

这种方法不仅使用了 $O(n)$ 的空间，且比第一种方法更差，因为在许多语言中，堆栈帧很大（如 Python），并且最大的运行时堆栈深度为 1000（可以增加，但是有可能导致底层解释程序内存出错）。为每个节点创建堆栈帧极大的限制了算法能够处理的最大链表大小。


