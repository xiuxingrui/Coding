# [LeetCode946.验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)
## 题目描述
给定 `pushed` 和 `popped` 两个序列，每个序列中的 **值都不重复**，只有当它们可能是在最初空栈上进行的推入 `push` 和弹出 `pop` 操作序列的结果时，返回 `true`；否则，返回 `false` 。

### 示例1
```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```
### 示例2
```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```
### 提示
1. `0 <= pushed.length == popped.length <= 1000`
2. `0 <= pushed[i], popped[i] < 1000`
3. `pushed` 是 `popped` 的排列。

## 题解
所有的元素一定是按顺序 `push` 进去的，重要的是怎么 `pop` 出来？

假设当前栈顶元素值为 2，同时对应的 `popped` 序列中下一个要 `pop` 的值也为 2，那就必须立刻把这个值 `pop` 出来。因为之后的 `push` 都会让栈顶元素变成不同于 2 的其他值，这样再 `pop` 出来的数 `popped` 序列就不对应了。

将 `pushed` 队列中的每个数都 `push` 到栈中，同时检查这个数是不是 `popped` 序列中下一个要 `pop` 的值，如果是就把它 `pop` 出来。

最后，检查不是所有的该 `pop` 出来的值都是 `pop` 出来了。

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Deque<Integer> stack = new LinkedList<>();
        int i = 0;
        for(int num : pushed) {
            stack.push(num); // num 入栈
            while(!stack.isEmpty() && stack.peek() == popped[i]) { // 循环判断与出栈
                stack.pop();
                i++;
            }
        }
        return stack.isEmpty();
    }
}
```
### 复杂度分析
时间复杂度：$O(N)$，其中 $N$ 是 `pushed` 序列和 `popped` 序列的长度。

空间复杂度：$O(N)$。

