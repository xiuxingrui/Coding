# [LeetCode138.复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
## 题目描述
给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的 **深拷贝**。 

我们用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：

- `val`：一个表示 `Node.val` 的整数。
- `random_index`：随机指针指向的节点索引（范围从 0 到 `n-1`）；如果不指向任何节点，则为  `null` 。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008163450.png)
```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008163510.png)
```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008163527.png)
```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```
```
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。
```
## 题解
### 解法一
用`HashMap`:
```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        HashMap<Node,Node> map=new HashMap<>();
        Node cur=head;
        while(cur!=null){
            map.put(cur,new Node(cur.val));
            cur=cur.next;
        }
        cur=head;
        while(cur!=null){
            map.get(cur).next=map.get(cur.next);
            map.get(cur).random=map.get(cur.random);
            cur=cur.next;
        }
        return map.get(head);
    }
}
```
### 解法二
这题的最大难点就在于复制随机指针，比如下图中

- 节点1的随机指针指向节点3
- 节点3的随机指针指向节点2
- 节点2的随机指针指向空

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008180315.png)

我们可以用三步走来搞定这个问题

第一步，根据遍历到的原节点创建对应的新节点，每个新创建的节点是在原节点后面，比如下图中原节点1不再指向原原节点2，而是指向新节点1

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008180339.png)

第二步是最关键的一步，用来设置新链表的随机指针

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008180356.png)

上图中，我们可以观察到这么一个规律

- 原节点1的随机指针指向原节点3，新节点1的随机指针指向的是原节点3的`next`
- 原节点3的随机指针指向原节点2，新节点3的随机指针指向的是原节点2的`next`

也就是，原节点`i`的随机指针(如果有的话)，指向的是原节点`j`

那么新节点`i`的随机指针，指向的是原节点`j`的`next`

第三步就简单了，只要将两个链表分离开，再返回新链表就可以了

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008180455.png)
整体思路：
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008181402.png)

1. 输入如下图 `[[1,2],[2,3],[3,null],[4,null]]`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008181535.png)

2. 复制源节点。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008181549.png)

3. 生成克隆节点的 `random` 指针。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008181608.png)

4. 将原链表和克隆链表分离。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008181623.png)


```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head==null) {
            return null;
        }
        Node p = head;
        //第一步，在每个原节点后面创建一个新节点
        //1->1'->2->2'->3->3'
        while(p!=null) {
            Node newNode = new Node(p.val);
            newNode.next = p.next;
            p.next = newNode;
            p = newNode.next;
        }
        p = head;
        //第二步，设置新节点的随机节点
        while(p!=null) {
            if(p.random!=null) {
                p.next.random = p.random.next;
            }
            p = p.next.next;
        }
        Node dummy = new Node(-1);
        p = head;
        Node cur = dummy;
        //第三步，将两个链表分离
        while(p!=null) {
            cur.next = p.next;
            cur = cur.next;
            p.next = cur.next;
            p = p.next;
        }
        return dummy.next;
    }
}	
```