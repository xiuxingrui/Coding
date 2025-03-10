# [LeetCode503.下一个更大元素II](https://leetcode-cn.com/problems/next-greater-element-ii/)
## 题目描述
给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 `x` 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。

 输入数组的长度不会超过 10000。

### 示例
```
输入: [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```
## 题解
###
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int len=nums.length;
        int[] temp=new int[len*2];
        for(int i=0;i<len*2;i++){
            temp[i]=nums[i%len];
        }
        Deque<Integer> stack=new LinkedList<>();
        int[] ans=new int[len*2];
        for(int i=len*2-1;i>=0;i--){
            while(!stack.isEmpty()&&stack.peek()<=temp[i]){
                stack.pollFirst();
            }
            if(stack.isEmpty()==false){
                ans[i]=stack.peek();
            }else{
                ans[i]=-1;
            }
            stack.offerFirst(temp[i]);
        }
        return Arrays.copyOf(ans,len);
    }
}
```
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int len=nums.length;
        Deque<Integer> stack=new LinkedList<>();
        int[] ans=new int[len];
        for(int i=len*2-1;i>=0;i--){
            while(!stack.isEmpty()&&stack.peek()<=nums[i%len]){
                stack.pollFirst();
            }
            if(stack.isEmpty()==false){
                ans[i%len]=stack.peek();
            }else{
                ans[i%len]=-1;
            }
            stack.offerFirst(nums[i%len]);
        }
        return ans;
    }
}
```