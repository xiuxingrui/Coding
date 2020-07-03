# [剑指Offer09.用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)
## 题目描述
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

### 示例1
```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```
### 示例2
```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```
### 提示
- `1 <= values <= 10000`
- 最多会对 `appendTail`、`deleteHead` 进行 10000 次调用

## 题解
### 解法一
```java
class CQueue {
    Deque<Integer> A, B;
    public CQueue() {
        A = new LinkedList<Integer>();
        B = new LinkedList<Integer>();
    }
    public void appendTail(int value) {
        A.offerLast(value);
    }
    public int deleteHead() {
        if(!B.isEmpty()) return B.pollLast();
        if(A.isEmpty()) return -1;
        while(!A.isEmpty())
            B.offerLast(A.pollLast());
        return B.pollLast();
    }
    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
}
}
/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```
#### 复杂度分析
时间复杂度：对于插入和删除操作，时间复杂度均为 $O(1)$。插入不多说，对于删除操作，虽然最坏情况下 $O(n)$ 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 `B` 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 $O(1)$。

空间复杂度：$O(n)$。需要使用两个栈存储已有的元素。


