# [LeetCode739.每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
## 题目描述
请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。

提示：气温 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。

## 题解
### 解法一
单调栈第一种写法:
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        Deque<Integer> stack=new LinkedList<>();
        int len=T.length;
        int[] ans=new int[len];
        for(int i=len-1;i>=0;i--){
            while(stack.isEmpty()==false&&T[stack.peek()]<=T[i]){
                stack.pollFirst();
            }
            if(stack.isEmpty()==true){
                ans[i]=0;
            }else{
                ans[i]=stack.peek()-i;
            }
            stack.offerFirst(i);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是温度列表的长度。正向遍历温度列表一遍，对于温度列表中的每个下标，最多有一次进栈和出栈的操作。
- 空间复杂度：$O(n)$，其中 `n` 是温度列表的长度。需要维护一个单调栈存储温度列表中的下标。
### 解法二
单调栈第二种写法

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924165248.gif)

可以维护一个存储下标的单调栈，从栈底到栈顶的下标对应的温度列表中的温度依次递减。如果一个下标在单调栈里，则表示尚未找到下一次温度更高的下标。

正向遍历温度列表。对于温度列表中的每个元素 `T[i]`，如果栈为空，则直接将 `i` 进栈，如果栈不为空，则比较栈顶元素 `prevIndex` 对应的温度 `T[prevIndex]` 和当前温度 `T[i]`，如果 `T[i] > T[prevIndex]`，则将 `prevIndex` 移除，并将 `prevIndex` 对应的等待天数赋为 `i - prevIndex`，重复上述操作直到栈为空或者栈顶元素对应的温度小于等于当前温度，然后将 `i` 进栈。

为什么可以在弹栈的时候更新 `ans[prevIndex]` 呢？因为在这种情况下，即将进栈的 `i` 对应的 `T[i]` 一定是 `T[prevIndex]` 右边第一个比它大的元素，试想如果 `prevIndex` 和 `i` 有比它大的元素，假设下标为 `j`，那么 `prevIndex` 一定会在下标 `j` 的那一轮被弹掉。

由于单调栈满足从栈底到栈顶元素对应的温度递减，因此每次有元素进栈时，会将温度更低的元素全部移除，并更新出栈元素对应的等待天数，这样可以确保等待天数一定是最小的。

以下用一个具体的例子帮助读者理解单调栈。对于温度列表 `[73,74,75,71,69,72,76,73]`，单调栈 stack\textit{stack}stack 的初始状态为空，答案 `ans` 的初始状态是 `[0,0,0,0,0,0,0,0]`，按照以下步骤更新单调栈和答案，其中单调栈内的元素都是下标，括号内的数字表示下标在温度列表中对应的温度。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924164834.png)

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int length = T.length;
        int[] ans = new int[length];
        Deque<Integer> stack = new LinkedList<Integer>();
        for (int i = 0; i < length; i++) {
            int temperature = T[i];
            while (!stack.isEmpty() && temperature > T[stack.peek()]) {
                int prevIndex = stack.pollFirst();
                ans[prevIndex] = i - prevIndex;
            }
            stack.offerFirst(i);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是温度列表的长度。正向遍历温度列表一遍，对于温度列表中的每个下标，最多有一次进栈和出栈的操作。
- 空间复杂度：$O(n)$，其中 `n` 是温度列表的长度。需要维护一个单调栈存储温度列表中的下标。
### 解法三
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        if(T==null){
            return new int[0];
        }
        int len=T.length;
        if(len==1){
            return new int[]{0};
        }
        int[] ans=new int[len];
        for(int i=0;i<len;i++){
            ans[i]=0;
            for(int j=i+1;j<len;j++){
                if(T[j]>T[i]){
                    ans[i]=j-i;
                    break;
                }
            }
        }
        return ans;
    }
}
```
