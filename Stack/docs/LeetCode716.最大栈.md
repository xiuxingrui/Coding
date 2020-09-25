# [LeetCode716.最大栈](https://leetcode-cn.com/problems/max-stack/)
## 题目描述
设计一个最大栈，支持 `push`、`pop`、`top`、`peekMax` 和 `popMax` 操作。

- `push(x)` -- 将元素 `x` 压入栈中。
- `pop()` -- 移除栈顶元素并返回这个值。
- `top()` -- 返回栈顶元素。
- `peekMax()` -- 返回栈中最大元素。
- `popMax()` -- 返回栈中最大的元素，并将其删除。如果有多个最大元素，只要删除最靠近栈顶的那个。

- `-1e7 <= x <= 1e7`
- 操作次数不会超过 10000。
- 当栈为空的时候不会出现后四个操作。
### 示例
```
MaxStack stack = new MaxStack();
stack.push(5); 
stack.push(1);
stack.push(5);
stack.top(); -> 5
stack.popMax(); -> 5
stack.top(); -> 1
stack.peekMax(); -> 5
stack.pop(); -> 1
stack.top(); -> 5
```
## 题解
### 解法一
一个普通的栈可以支持前三种操作 `push(x)`，`pop()` 和 `top()`，所以我们需要考虑的仅为后两种操作 `peekMax()` 和 `popMax()`。

对于 `peekMax()`，我们可以另一个栈来存储每个位置到栈底的所有元素的最大值。例如，如果当前第一个栈中的元素为 `[2, 1, 5, 3, 9]`，那么第二个栈中的元素为 `[2, 2, 5, 5, 9]`。在 `push(x)` 操作时，只需要将第二个栈的栈顶和 `x` 的最大值入栈，而在 `pop()` 操作时，只需要将第二个栈进行出栈。

对于 `popMax()`，由于我们知道当前栈中最大的元素值，因此可以直接将两个栈同时出栈，并存储第一个栈出栈的所有值。当某个时刻，第一个栈的出栈元素等于当前栈中最大的元素值时，就找到了最大的元素。此时我们将之前出第一个栈的所有元素重新入栈，并同步更新第二个栈，就完成了 `popMax()` 操作。

```java
class MaxStack {
    Deque<Integer> stack;
    Deque<Integer> maxStack;

    public MaxStack() {
        stack = new LinkedList<>();
        maxStack = new LinkedList<>();
    }

    public void push(int x) {
        int max = maxStack.isEmpty() ? x : maxStack.peek();
        maxStack.offerFirst(max > x ? max : x);
        stack.offerFirst(x);
    }

    public int pop() {
        maxStack.pollFirst();
        return stack.pollFirst();
    }

    public int top() {
        return stack.peek();
    }

    public int peekMax() {
        return maxStack.peek();
    }

    public int popMax() {
        int max = peekMax();
        Deque<Integer> buffer = new LinkedList<>();
        while (top() != max) buffer.offerFirst(pop());
        pop();
        while (!buffer.isEmpty()) push(buffer.pollFirst());
        return max;
    }
}
```

### 解法二
借鉴最小栈：
```java
class MaxStack {
    Deque<Integer> stack;
    Deque<Integer> max;
    /** initialize your data structure here. */
    public MaxStack() {
        stack=new LinkedList<>();
        max=new LinkedList<>();
    }
    
    public void push(int x) {
        stack.offerFirst(x);
        if(max.isEmpty()||x>=max.peek()){
            max.offerFirst(x);
        }
    }
    
    public int pop() {
        int ans=stack.pollFirst();
        if(max.peek().equals(ans)){
            max.pollFirst();
        }
        return ans;
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int peekMax() {
        return max.peek();
    }
    
    public int popMax() {
        int maxValue=max.pollFirst();
        Deque<Integer> tempStack=new LinkedList<>();
        while(stack.peek()!=maxValue){
            tempStack.offerFirst(stack.pollFirst());
        }
        stack.pollFirst();
        while(!tempStack.isEmpty()){
            stack.offerFirst(tempStack.pollFirst());
        }
        return maxValue;
    }
}

/**
 * Your MaxStack object will be instantiated and called as such:
 * MaxStack obj = new MaxStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.peekMax();
 * int param_5 = obj.popMax();
 */
```