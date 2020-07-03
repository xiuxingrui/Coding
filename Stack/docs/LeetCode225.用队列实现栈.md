# [LeetCode225.用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)
## 题目描述
使用队列实现栈的下列操作：

- `push(x)` -- 元素 `x` 入栈
- `pop()` -- 移除栈顶元素
- `top()` -- 获取栈顶元素
- `empty()` -- 返回栈是否为空

### 注意
1. 你只能使用队列的基本操作-- 也就是 `push to back`, `peek/pop from front`, `size`, 和 `is empty` 这些操作是合法的。
2. 你所使用的语言也许不支持队列。 你可以使用 `list` 或者 `deque`（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
3. 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 `pop` 或者 `top` 操作）。

## 题解
### 解法一
```java
class MyStack {
    Queue<Integer> q;
    /** Initialize your data structure here. */
    public MyStack() {
        q=new LinkedList<Integer>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        q.offer(x);
        int size=q.size();
        while(size>1){
            q.offer(q.poll());
            --size;
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return q.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return q.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```
#### 复杂度分析
时间复杂度：除了`push`操作为$O(n)$，其余操作均为$O(1)$。

空间复杂度:均为$O(1)$。


