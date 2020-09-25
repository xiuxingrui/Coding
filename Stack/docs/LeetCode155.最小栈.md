# [LeetCode155.最小栈](https://leetcode-cn.com/problems/min-stack/)
## 题目描述
设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

- `push(x)` —— 将元素 `x` 推入栈中。
- `pop()` —— 删除栈顶的元素。
- `top()` —— 获取栈顶元素。
- `getMin()` —— 检索栈中的最小元素。

- `pop`、`top` 和 `getMin` 操作总是在 非空栈 上调用。
### 示例
```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```
## 题解
### 解法一
对于栈来说，如果一个元素 `a` 在入栈时，栈里有其它的元素 `b`, `c`, `d`，那么无论这个栈在之后经历了什么操作，只要 `a` 在栈中，`b`, `c`, `d` 就一定在栈中，因为在 `a` 被弹出之前，`b`, `c`, `d` 不会被弹出。

因此，在操作过程中的任意一个时刻，只要栈顶的元素是 `a`，那么我们就可以确定栈里面现在的元素一定是 `a`, `b`, `c`, `d`。

那么，我们可以在每个元素 `a` 入栈时把当前栈的最小值 `m` 存储起来。在这之后无论何时，如果栈顶元素是 `a`，我们就可以直接返回存储的最小值 `m`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924210605.gif)

按照上面的思路，我们只需要设计一个数据结构，使得每个元素 `a` 与其相应的最小值 `m` 时刻保持一一对应。因此我们可以使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值。

1. 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；
2. 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；
3. 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中。

```java
class MinStack {
    Deque<Integer> xStack;
    Deque<Integer> minStack;

    public MinStack() {
        xStack = new LinkedList<Integer>();
        minStack = new LinkedList<Integer>();
        minStack.push(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        xStack.push(x);
        minStack.push(Math.min(minStack.peek(), x));
    }
    
    public void pop() {
        xStack.pop();
        minStack.pop();
    }
    
    public int top() {
        return xStack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```
#### 复杂度分析
- 时间复杂度：对于题目中的所有操作，时间复杂度均为 $O(1)$。因为栈的插入、删除与读取操作都是 $O(1)$，我们定义的每个操作最多调用栈操作两次。
- 空间复杂度：$O(n)$，其中 `n` 为总操作数。最坏情况下，我们会连续插入 `n` 个元素，此时两个栈占用的空间为 $O(n)$。

### 解法二
本题难点： 将 `min()` 函数复杂度降为 $O(1)$ ，可通过建立辅助栈实现；
- 数据栈 `A` ： 栈 `A` 用于存储所有元素，保证入栈 $push()$ 函数、出栈 $pop()$ 函数、获取栈顶 $top()$ 函数的正常逻辑。
- 辅助栈 `B` ： 栈 `B` 中存储栈 `A` 中所有 非严格降序 的元素，则栈 `A` 中的最小元素始终对应栈 `B` 的栈顶元素，即 $min()$ 函数只需返回栈 `B` 的栈顶元素即可。

因此，只需设法维护好 栈 `B` 的元素，使其保持非严格降序，即可实现 $min()$ 函数的 $O(1)$ 复杂度。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924184812.png)

- `push(x)` 函数： 重点为保持栈 `B` 的元素是 非严格降序 的。

  - 将 `x` 压入栈 `A` （即 `A.offerFirst(x)` ）；
  - 若 ① 栈 `B` 为空 或 ② `x` 小于等于 栈 `B` 的栈顶元素，则将 `x` 压入栈 `B` （即 `B.offerFirst(x)` ）。

- `pop()` 函数： 重点为保持栈 `A`,`B` 的 元素一致性 。 
  - 执行栈 `A` 出栈（即 `A.pollFirst()`），将出栈元素记为 `y` ；
  - 若 `y` 等于栈 `B` 的栈顶元素，则执行栈 `B` 出栈（即 `B.pollFirst()` ）。  
- `top()` 函数： 直接返回栈 `A` 的栈顶元素即可，即返回 `A.peek()` 。
- `getMin()` 函数： 直接返回栈 `B` 的栈顶元素即可，即返回 `B.peek()` 。
边界条件：
1. 辅助栈为空的时候，必须放入新进来的数；
2. 新来的数小于或者等于辅助栈栈顶元素的时候，才放入，特别注意这里“等于”要考虑进去，因为出栈的时候，连续的、相等的并且是最小值的元素要同步出栈；
3. 出栈的时候，辅助栈的栈顶元素等于数据栈的栈顶元素，才出栈。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924184419.gif)

```java
class MinStack {
    Deque<Integer> A, B;
    public MinStack() {
        A = new LinkedList<>();
        B = new LinkedList<>();
    }
    public void push(int x) {
        A.offerFirst(x);
        if(B.isEmpty()|| B.peek() >=x)
            B.offerFirst(x);
    }
    public void pop() {
        if(A.pollFirst().equals(B.peek()))
            B.pollFirst();
    }
    public int top() {
        return A.peek();
    }
    public int getMin() {
        return B.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```
### 复杂度分析：
- 时间复杂度 $O(1)$ ： `push()`, `pop()`, `top()`, `min()` 四个函数的时间复杂度均为常数级别。
- 空间复杂度 $O(N)$： 当共有 `N` 个待入栈元素时，辅助栈 `B` 最差情况下存储 `N` 个元素，使用 $O(N)$ 额外空间。

