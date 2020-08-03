# [LeetCode109.有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)
## 题目描述
给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

### 示例
```
给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```
## 题解
### 解法三
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    ListNode cur;
    public TreeNode sortedListToBST(ListNode head) {
        int end=0;
        cur=head;
        while(head!=null){
            head=head.next;
            end++;
        }
        return sortedListToBSTHelper(0,end-1);
    }
    public TreeNode sortedListToBSTHelper(int start,int end){
        if(start>end){
            return null;
        }
        int mid=start+(end-start)/2;
        TreeNode left=sortedListToBSTHelper(start,mid-1);
        TreeNode root=new TreeNode(cur.val);
        cur=cur.next;
        TreeNode right=sortedListToBSTHelper(mid+1,end);
        root.left=left;
        root.right=right;
        return root;
    }
}
```
#### 复杂度分析
- 时间复杂度：时间复杂度仍然为 $O(N)$ 因为我们需要遍历链表中所有的顶点一次并构造相应的二叉搜索树节点。
- 空间复杂度：$O(logN)$，额外空间只有一个递归栈，由于是一棵高度平衡的二叉搜索树，所以高度上界为 $O(logN)$。
### 解法二
给定列表中的中间元素将会作为二叉搜索树的根，该点左侧的所有元素递归的去构造左子树，同理右侧的元素构造右子树。这必然能够保证最后构造出的二叉搜索树是平衡的。

```java
public TreeNode sortedListToBST(ListNode head) {
    return sortedArrayToBST(head, null);
}

private TreeNode sortedArrayToBST(ListNode head, ListNode tail) {
    if (head == tail) {
        return null;
    }
    ListNode fast = head;
    ListNode slow = head;
    while (fast != tail && fast.next != tail) {
        slow = slow.next;
        fast = fast.next.next;
    }

    TreeNode root = new TreeNode(slow.val);
    root.left = sortedArrayToBST(head, slow);
    root.right = sortedArrayToBST(slow.next, tail); 
    return root;
}
```
写法二:
1. 由于我们得到的是一个有序链表而不是数组，我们不能直接使用下标来访问元素。我们需要知道链表中的中间元素。
2. 我们可以利用两个指针来访问链表中的中间元素。假设我们有两个指针 `slow_ptr` 和 `fast_ptr`。`slow_ptr` 每次向后移动一个节点而 `fast_ptr` 每次移动两个节点。当 `fast_ptr` 到链表的末尾时 `slow_ptr` 就访问到链表的中间元素。对于一个偶数长度的数组，中间两个元素都可用来作二叉搜索树的根。
3. 当找到链表中的中间元素后，我们将链表从中间元素的左侧断开，做法是使用一个 `prev_ptr` 的指针记录 `slow_ptr` 之前的元素，也就是满足 `prev_ptr.next = slow_ptr`。断开左侧部分就是让 `prev_ptr.next = None`。
4. 我们只需要将链表的头指针传递给转换函数，进行高度平衡二叉搜索树的转换。所以递归调用的时候，左半部分我们传递原始的头指针；右半部分传递 `slow_ptr.next` 作为头指针。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        // If the head doesn't exist, then the linked list is empty
        if (head == null) {
            return null;
        }
        // Find the middle element for the list.
        ListNode mid = findMiddleElement(head);
        // The mid becomes the root of the BST.
        TreeNode node = new TreeNode(mid.val);
        // Base case when there is just one element in the linked list
        if (head == mid) {
            return node;
        }
        // Recursively form balanced BSTs using the left and right halves of the original list.
        node.left = this.sortedListToBST(head);
        node.right = this.sortedListToBST(mid.next);
        return node;
    }
    public ListNode findMiddleElement(ListNode head) {
        // The pointer used to disconnect the left half from the mid node.
        ListNode prevPtr = null;
        ListNode slowPtr = head;
        ListNode fastPtr = head;
        // Iterate until fastPr doesn't reach the end of the linked list.
        while (fastPtr != null && fastPtr.next != null) {
        prevPtr = slowPtr;
        slowPtr = slowPtr.next;
        fastPtr = fastPtr.next.next;
        }
        // Handling the case when slowPtr was equal to head.
        if (prevPtr != null) {
        prevPtr.next = null;
        }
        return slowPtr;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(NlogN)$。假设链表包含 `N` 个元素，对于传入递归函数的每个列表，我们需要计算它的中间元素。对于一个大小为 `N` 的列表，需要 `N/2`步找到中间元素，也就是花费 $O(N)$ 的时间。我们对于原始数组的每一半都要同样的操作，看上去这是一个$O(N^2)$的算法，但仔细分析会发现比 $O(N^2)$更高效。

统计一下每一半列表中所需要的操作数，根据上文所述对于 `N` 个元素我们需要 `N/2`步得到中间元素。当找到中间元素之后我们将剩下两个大小为 `N/2` 的子列表，对这两个部分都需要找中间元素的操作，需要时间开销为 `2×N/4` 步，同理对于更小的列表也是递归的操作。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200803170249.png)

显然，每次将列表分成一半，需要 $logN$ 的时间结束。因此，上面的等式可写成：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200803170340.png)

- 空间复杂度：$O(logN)$。因为使用递归的方法，所以需要考虑递归栈的空间复杂度。对于一棵费平衡二叉树，可能需要 $O(N)$ 的空间，但是问题描述中要求维护一棵平衡二叉树，所以保证树的高度上界为 $O(logN)$，因此空间复杂度为 $O(logN)$。

### 解法三
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        ArrayList<Integer> nums = new ArrayList<>();
        while (head != null) {
            nums.add(head.val);
            head = head.next;
        }
        return sortedArrayToBST(nums);
    }

    public TreeNode sortedArrayToBST(ArrayList<Integer> nums) {
        return sortedArrayToBST(nums, 0, nums.size()-1);
    }

    public TreeNode sortedArrayToBST(ArrayList<Integer> nums, int start, int end) {
        if (start>end) {
            return null;
        }
        int mid = start+(end-start)/2;
        TreeNode root = new TreeNode(nums.get(mid));
        root.left = sortedArrayToBST(nums, start, mid-1);
        root.right = sortedArrayToBST(nums, mid + 1, end);
        return root;
    }
}
```
#### 复杂度分析
- 时间复杂度：时间复杂度降到了 $O(N)$，因为需要将链表转成数组。而取中间元素的开销变成了 $O(1)$ 所以整体的时间复杂度降低了。
- 空间复杂度：因为我们利用额外空间换取了时间复杂度的降低，空间复杂度变成了 $O(N)$，相较于之前算法的 $​O(logN)$​ 有所提升，因为创建数组的开销。

