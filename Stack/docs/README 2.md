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
```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int len=heights.length;
        int[] leftMin=new int[len];
        int[] rightMin=new int[len];
        Deque<Integer> stack=new LinkedList<>();
        for(int i=len-1;i>=0;i--){
            while(!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                stack.pollFirst();
            }
            if(stack.isEmpty()==false){
                rightMin[i]=stack.peek();
            }else{
                rightMin[i]=-1;
            }
            stack.offerFirst(i);
        }
        stack.clear();
        for(int i=0;i<len;i++){
            while(!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                stack.pollFirst();
            }
            if(stack.isEmpty()==false){
                leftMin[i]=stack.peek();
            }else{
                leftMin[i]=-1;
            }
            stack.offerFirst(i);
        }
        int ans=0;
        for(int i=0;i<len;i++){
            if(leftMin[i]==-1){
                if(rightMin[i]==-1){
                    ans=ans>len*heights[i]?ans:len*heights[i];
                }else{
                    ans=ans>(rightMin[i])*heights[i]?ans:(rightMin[i])*heights[i];
                }
            }else{
                if(rightMin[i]==-1){
                    ans=ans>(len-leftMin[i]-1)*heights[i]?ans:(len-leftMin[i]-1)*heights[i];
                }else{
                    ans=ans>(rightMin[i]-1-leftMin[i]-1+1)*heights[i]?ans:(rightMin[i]-1-leftMin[i]-1+1)*heights[i];
                }
            }
        }
        return ans;
    }
}
```

