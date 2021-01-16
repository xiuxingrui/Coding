# [LeetCode232.用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)
## 题目描述
请你仅使用两个栈实现先入先出队列。队列应当支持一般队列的支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)`: 将元素 `x` 推到队列的末尾
- `int pop()`: 从队列的开头移除并返回元素
- `int peek()`: 返回队列开头的元素
- `boolean empty()`: 如果队列为空，返回 `true` ；否则，返回 `false`
 

说明：

- 你只能使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。

- 你所使用的语言也许不支持栈。你可以使用 `list` 或者 `deque`（双端队列）来模拟一个栈，只要是标准的栈操作即可。
 

进阶：

- 你能否实现每个操作均摊时间复杂度为 $O(1)$ 的队列？换句话说，执行 `n` 个操作的总时间复杂度为 $O(n)$ ，即使其中一个操作可能花费较长时间。

### 示例
```
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```
### 提示
- `1 <= x <= 9`
- 最多调用 100 次 `push`、`pop`、`peek` 和 `empty`
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 `pop` 或者 `peek` 操作）
## 题解
维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，我们每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第二个栈，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。如果为空，我们将第一个栈里的元素一个个弹出插入到第二个栈里，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。

维护两个栈 `stack1` 和 `stack2`，其中 `stack1` 支持插入操作，`stack2` 支持删除操作

构造方法:

- 初始化 `stack1` 和 `stack2` 为空

插入元素:

- `stack1` 直接插入元素

删除元素:
- 如果 `stack2` 为空，则将 `stack1` 里的所有元素弹出插入到 `stack2` 里
- 如果 `stack2` 仍为空，则返回 -1，否则从 `stack2` 弹出一个元素并返回

```java
class MyQueue {
    Deque<Integer> stack1,stack2;

    /** Initialize your data structure here. */
    public MyQueue() {
        stack1=new LinkedList<>();
        stack2=new LinkedList<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        stack1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(!stack2.isEmpty()){
            return stack2.pop();
        }else{
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
            return stack2.pop();
        }
    }
    
    /** Get the front element. */
    public int peek() {
        if(!stack2.isEmpty()){
            return stack2.peek();
        }else{
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
            return stack2.peek();
        }
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack1.isEmpty()&&stack2.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```
### 复杂度分析
时间复杂度：对于插入和删除操作，时间复杂度均为 $O(1)$。插入不多说，对于删除操作，虽然最坏情况下 $O(n)$ 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 `B` 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 $O(1)$。

空间复杂度：$O(n)$。需要使用两个栈存储已有的元素。
