# [LeetCode23.合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
## 题目描述
合并 `k` 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

### 示例
```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
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
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists == null||lists.length==0){
            return null;
        }
        return mergeKListsHelper(lists,0,lists.length-1);
    }
    public ListNode mergeKListsHelper(ListNode[] lists,int start,int end){
        if(start>end){
            return null;
        }
        if(start==end){
            return lists[start];
        }
        int mid=start+(end-start)/2;
        ListNode tempList1=mergeKListsHelper(lists,start,mid);
        ListNode tempList2=mergeKListsHelper(lists,mid+1,end);
        return mergeTwoLists(tempList1,tempList2);
    }
    public ListNode mergeTwoLists(ListNode list1,ListNode list2){
        if(list1==null){
            return list2;
        }
        if(list2==null){
            return list1;
        }
        ListNode dummyHead=new ListNode(0);
        ListNode tail=dummyHead;
        while(list1!=null&&list2!=null){
            if(list1.val<=list2.val){
                tail.next=list1;
                tail=list1;
                list1=list1.next;
            }else{
                tail.next=list2;
                tail=list2;
                list2=list2.next;
            }
        }
        if(list1!=null){
            tail.next=list1;
        }
        if(list2!=null){
            tail.next=list2;
        }
        return dummyHead.next;
    }
}
```
递归写法：
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
   public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        return merge(lists, 0, lists.length - 1);
    }

    private ListNode merge(ListNode[] lists, int left, int right) {
        if (left == right) return lists[left];
        int mid = left + (right - left) / 2;
        ListNode l1 = merge(lists, left, mid);
        ListNode l2 = merge(lists, mid + 1, right);
        return mergeTwoLists(l1, l2);
    }

    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：考虑递归「向上回升」的过程——第一轮合并 `k/2`个链表，每一组的时间代价是 $O(2n)$；第二轮合并`k/4`组链表，每一组的时间代价是 $O(4n)$...所以总的时间代价是$O\left(\sum_{i=1}^{\infty} \frac{k}{2^{i}} \times 2^{i} n\right)=O(k n \times \log k)$,故渐进时间复杂度为 $O(kn×logk)$。(n为每个链表长度)
- 空间复杂度：递归会使用到 $O(logk)$ 空间代价的栈空间。
##### 粗暴解法
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
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0){
            return null;
        }
        ListNode ans=lists[0];
        for(int i=1;i<lists.length;i++){
            ans=mergeTwoLists(ans,lists[i]);
        }
        return ans;
    }
    public ListNode mergeTwoLists(ListNode list1,ListNode list2){
        if(list1==null){
            return list2;
        }
        if(list2==null){
            return list1;
        }
        ListNode dummyHead=new ListNode(0);
        ListNode tail=dummyHead;
        while(list1!=null&&list2!=null){
            if(list1.val<=list2.val){
                tail.next=list1;
                tail=list1;
                list1=list1.next;
            }else{
                tail.next=list2;
                tail=list2;
                list2=list2.next;
            }
        }
        if(list1!=null){
            tail.next=list1;
        }
        if(list2!=null){
            tail.next=list2;
        }
        return dummyHead.next;
    }
}
```
##### 复杂度分析
- 时间复杂度：假设每个链表的最长长度是 `n`。在第一次合并后，`ans` 的长度为 `n`；第二次合并后，`ans` 的长度为`2×n`，第 `i` 次合并后，`ans` 的长度为 `i×n`。第 `i` 次合并的时间代价是 $O(n+(i−1)×n)=O(i×n)$,那么总的时间代价为$O\left(\sum_{i=1}^{k}(i \times n)\right)=O\left(\frac{(1+k) \cdot k}{2} \times n\right)=O\left(k^{2} n\right)$,故渐进时间复杂度为$O\left(k^{2} n\right)$。
- 空间复杂度：没有用到与 `k` 和 `n` 规模相关的辅助空间，故渐进空间复杂度为 $O(1)$。

### 解法二
每次 $O(K)$ 比较 `K` 个指针求 `min`, 时间复杂度：$O(k^2n)$
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) { 
        int k = lists.length;
        ListNode dummyHead = new ListNode(0);
        ListNode tail = dummyHead;
        while (true) {
            ListNode minNode = null;
            int minPointer = -1;
            for (int i = 0; i < k; i++) {
                if (lists[i] == null) {
                    continue;
                }
                if (minNode == null || lists[i].val < minNode.val) {
                    minNode = lists[i];
                    minPointer = i;
                }
            }
            if (minPointer == -1) {
                break;
            }
            tail.next = minNode;
            tail = tail.next;
            lists[minPointer] = lists[minPointer].next;
        }
        return dummyHead.next;
    }
}
```
使用小根堆进行优化，每次 $O(logK)$ 比较 `K` 个指针求 `min`, 时间复杂度：$O(knlogk)$
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        Queue<ListNode> pq = new PriorityQueue<>((v1, v2) -> v1.val - v2.val);
        for (ListNode node: lists) {
            if (node != null) {
                pq.offer(node);
            }
        }

        ListNode dummyHead = new ListNode(0);
        ListNode tail = dummyHead;
        while (!pq.isEmpty()) {
            ListNode minNode = pq.poll();
            tail.next = minNode;
            tail = minNode;
            if (minNode.next != null) {
                pq.offer(minNode.next);
            }
        }

        return dummyHead.next;
    }
}
```
#### 复杂度分析
- 时间复杂度：考虑优先队列中的元素不超过 `k` 个，那么插入和删除的时间代价为 $O(logk)$，这里最多有 `kn` 个点，对于每个点都被插入删除各一次，故总的时间代价即渐进时间复杂度为 $O(kn×logk)$
- 空间复杂度：这里用了优先队列，优先队列中的元素不超过 `k` 个，故渐进空间复杂度为 $O(k)$。
