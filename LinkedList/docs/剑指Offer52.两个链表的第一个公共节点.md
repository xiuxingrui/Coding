# [剑指Offer52.两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)
## 题目描述
编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200701201506.png)

在节点 `c1` 开始相交。

### 示例1

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200701201552.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

### 示例2

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200701201627.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

### 示例3

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200701201700.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```
### 注意

1. 如果两个链表没有交点，返回 `null`.
2. 在返回结果后，两个链表仍须保持原有的结构。
3. 可假定整个链表结构中没有循环。
4. 程序尽量满足 $O(n)$ 时间复杂度，且仅用 $O(1)$ 内存。

## 题解
### 解法一:
我们还可以使用两个指针，最开始的时候一个指向链表A，一个指向链表B，然后他们每次都要往后移动一位，顺便查看节点是否相等。如果链表A和链表B不相交，基本上没啥可说的，我们这里假设链表A和链表B相交。那么就会有两种情况，

一种是链表A的长度和链表B的长度相等，他们每次都走一步，最终在相交点肯定会相遇。

一种是链表A的长度和链表B的长度不相等，如下图所示

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201219183941.png)

虽然他们有交点，但他们的长度不一样，所以他们完美的错开了，即使把链表都走完了也找不到相交点。

我们仔细看下上面的图，如果A指针把链表A走完了，然后再从链表B开始走到相遇点就相当于把这两个链表的所有节点都走了一遍，同理如果B指针把链表B走完了，然后再从链表A开始一直走到相遇点也相当于把这两个链表的所有节点都走完了

所以如果A指针走到链表末尾，下一步就让他从链表B开始。同理如果B指针走到链表末尾，下一步就让他从链表A开始。只要这两个链表相交最终肯定会在相交点相遇，如果不相交，最终他们都会同时走到两个链表的末尾，我们来画个图看一下

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201219184052.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201219184102.png)

如上图所示，A指针和B指针如果一直走下去，那么他们最终会在相交点相遇。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200701211458.png)
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode ha = headA, hb = headB;
        while (ha != hb) {
            ha = ha != null ? ha.next : headB;
            hb = hb != null ? hb.next : headA;
        }
        return ha;
    }
}
```
### 解法二
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA=0,lenB=0;
        ListNode tempA=headA,tempB=headB;
        while(tempA!=null){
            ++lenA;
            tempA=tempA.next;
        }
        while(tempB!=null){
            ++lenB;
            tempB=tempB.next;
        }
        int dis=Math.abs(lenA-lenB);
        if(lenA>=lenB){
            while(dis!=0){
                headA=headA.next;
                --dis;
            }
        }
        else{
            while(dis!=0){
                headB=headB.next;
                --dis;
            }
        }
        while(headA!=headB){
            headA=headA.next;
            headB=headB.next;
        }
        return headA;
    }
}
```


