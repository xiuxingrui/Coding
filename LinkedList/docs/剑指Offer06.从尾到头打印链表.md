# [剑指Offer06.从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)
## 题目描述

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

### 示例
```
输入：head = [1,3,2]
输出：[2,3,1]
```
## 题解
### 解法一
递归
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
    public int[] reversePrint(ListNode head) {
        List<Integer> list=new ArrayList<>();
        helper(list,head);
        int len=list.size();
        int[] res=new int[len];
        int cnt=0;
        for(int num:list){
            res[cnt++]=num;
        }
        return res;
    }
    public void helper(List<Integer> list,ListNode head){
        if(head==null){
            return;
        }
        helper(list,head.next);
        list.add(head.val);
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(N)$： 遍历链表，递归 `N` 次。
- 空间复杂度 $O(N)$： 系统递归需要使用 $O(N)$ 的栈空间。

### 解法二
栈
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
    public int[] reversePrint(ListNode head) {
        Deque<Integer> stack=new LinkedList<>();
        while(head!=null){
            stack.offerLast(head.val);
            head=head.next;
        }
        int[] res=new int[stack.size()];
        int cnt=0;
        while(!stack.isEmpty()){
            res[cnt++]=stack.pollLast();
        }
        return res;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$。正向遍历一遍链表，然后从栈弹出全部节点，等于又反向遍历一遍链表。
- 空间复杂度：$O(n)$。额外使用一个栈存储链表中的每个节点。




