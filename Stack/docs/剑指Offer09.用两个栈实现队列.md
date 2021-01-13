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
维护两个栈，第一个栈支持删除操作，第二个栈支持插入操作。

根据栈先进后出的特性，我们每次往第二个栈里插入元素后，第二个栈的底部元素是最后插入的元素，第二个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第一个栈，用第一个栈维护待删除的元素，在执行删除操作的时候我们首先看下第一个栈是否为空。如果为空，我们将第二个栈里的元素一个个弹出插入到第一个栈里，这样第一个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第一个栈的元素返回即可。

成员变量:

维护两个栈 `stack1` 和 `stack2`，其中 `stack1` 支持删除操作，`stack2` 支持插入操作

构造方法:

初始化 `stack1` 和 `stack2` 为空

插入元素:

- `stack2` 直接插入元素

删除元素:

- 如果 `stack1` 为空，则将 `stack2` 里的所有元素弹出插入到 `stack1` 里
- 从 `stack2` 弹出一个元素并返回

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
        stack2.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(!stack1.isEmpty()){
            return stack1.pop();
        }else{
            while(!stack2.isEmpty()){
                stack1.push(stack2.pop());
            }
            return stack1.pop();
        }
    }
    
    /** Get the front element. */
    public int peek() {
        if(!stack1.isEmpty()){
            return stack1.peek();
        }else{
            while(!stack2.isEmpty()){
                stack1.push(stack2.pop());
            }
            return stack1.peek();
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


