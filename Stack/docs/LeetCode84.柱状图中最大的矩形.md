# [LeetCode84.柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)
## 题目描述
给定 `n` 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200916183158.png)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 `[2,1,5,6,2,3]`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200916183216.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。

### 示例
```
输入: [2,1,5,6,2,3]
输出: 10
```
## 题解
### 解法一
依次遍历柱形的高度，对于每一个高度分别向两边扩散，求出以当前高度为矩形的最大宽度多少。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028163207.png)

为此，我们需要：

左边看一下，看最多能向左延伸多长，找到大于等于当前柱形高度的最左边元素的下标；

右边看一下，看最多能向右延伸多长；找到大于等于当前柱形高度的最右边元素的下标。

对于每一个位置，我们都这样操作，得到一个矩形面积，求出它们的最大值。

暴力法：

```java
public class Solution {

    public int largestRectangleArea(int[] heights) {
        int len = heights.length;
        // 特判
        if (len == 0) {
            return 0;
        }

        int res = 0;
        for (int i = 0; i < len; i++) {

            // 找左边最后 1 个大于等于 heights[i] 的下标
            int left = i;
            while (left >=0 && heights[left] >= heights[i]) {
                left--;
            }
            // 找右边最后 1 个大于等于 heights[i] 的索引
            int right = i;
            while (right<len&& heights[right] >=heights[i]) {
                right++;
            }

            int width = right - left- 1;
            res = Math.max(res, width * heights[i]);
        }
        return res;
    }
}

```
#### 复杂度分析
- 时间复杂度：$O(N^2)$，这里 `N` 是输入数组的长度。
- 空间复杂度：$O(1)$。

### 解法二
单调栈：



```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int len=heights.length;
        int[] leftMin=new int[len];
        int[] rightMin=new int[len];
        Deque<Integer> stack=new LinkedList<>();
        for(int i=len-1;i>=0;i--){
            while(!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                stack.pop();
            }
            if(stack.isEmpty()==false){
                rightMin[i]=stack.peek();
            }else{
                rightMin[i]=len;
            }
            stack.push(i);
        }
        stack.clear();
        for(int i=0;i<len;i++){
            while(!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                stack.pop();
            }
            if(stack.isEmpty()==false){
                leftMin[i]=stack.peek();
            }else{
                leftMin[i]=-1;
            }
            stack.push(i);
        }
        int ans=0;
        for(int i=0;i<len;i++){
            ans=Math.max(ans,(rightMin[i]-leftMin[i]-1)*heights[i]);
        }
        return ans;
    }
}
```
我们在对位置 `i` 进行入栈操作时，确定了它的左边界。从直觉上来说，与之对应的我们在对位置 `i` 进行出栈操作时可以确定它的右边界！仔细想一想，这确实是对的。当位置 `i` 被弹出栈时，说明此时遍历到的位置 `i0`的高度小于等于 `height[i]`，并且在 `i0`与 `i` 之间没有其他高度小于等于 `height[i]` 的柱子。这是因为，如果在 `i` 和 `i0`之间还有其它位置的高度小于等于 `height[i]` 的，那么在遍历到那个位置的时候，`i` 应该已经被弹出栈了。所以位置 `i0`就是位置 `i` 的右边界。

但是我们需要的是「一根柱子的左侧且最近的小于其高度的柱子」，但这里我们求的是小于等于，那么会造成什么影响呢？答案是：我们确实无法求出正确的右边界，但对最终的答案没有任何影响。这是因为在答案对应的矩形中，如果有若干个连续的柱子的高度都等于矩形的高度，那么最右侧的那根柱子是可以求出正确的右边界的，而我们没有对求出左边界的算法进行任何改动，因此最终的答案还是可以从最右侧的与矩形高度相同的柱子求得的。

在遍历结束后，栈中仍然有一些位置，这些位置对应的右边界就是位置为 `n` 的「哨兵」。我们可以将它们依次出栈并更新右边界，也可以在初始化右边界数组时就将所有的元素的值置为 `n`。

写法一：
```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(right, n);
        
        Deque<Integer> mono_stack = new LinkedList<>();
        for (int i = 0; i < n; ++i) {
            while (!mono_stack.isEmpty() && heights[mono_stack.peek()] >= heights[i]) {
                right[mono_stack.peek()] = i;
                mono_stack.pop();
            }
            left[i] = (mono_stack.isEmpty() ? -1 : mono_stack.peek());
            mono_stack.push(i);
        }
        
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, (right[i] - left[i] - 1) * heights[i]);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$。
- 空间复杂度：$O(N)$。
