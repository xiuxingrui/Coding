# [LeetCode496.下一个更大元素I](https://leetcode-cn.com/problems/next-greater-element-i/)
## 题目描述
给定两个 没有重复元素 的数组 `nums1` 和 `nums2` ，其中`nums1` 是 `nums2` 的子集。找到 `nums1` 中每个元素在 `nums2` 中的下一个比其大的值。

`nums1` 中数字 `x` 的下一个更大元素是指 `x` 在 `nums2` 中对应位置的右边的第一个比 `x` 大的元素。如果不存在，对应位置输出 -1 。

- `nums1`和`nums2`中所有元素是唯一的。
- `nums1`和`nums2` 的数组大小都不超过1000。
### 示例
```
输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。
```
```
输入: nums1 = [2,4], nums2 = [1,2,3,4].
输出: [3,-1]
解释:
    对于 num1 中的数字 2 ，第二个数组中的下一个较大数字是 3 。
    对于 num1 中的数字 4 ，第二个数组中没有下一个更大的数字，因此输出 -1 。
```
## 题解
### 解法一
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        if(nums1==null||nums1.length==0){
            return new int[0];
        }
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<nums2.length;i++){
            map.put(nums2[i],i);
        }
        int[] ans=new int[nums1.length];
        int[] temp=new int[nums2.length];
        Deque<Integer> stack=new LinkedList<>();
        for(int i=nums2.length-1;i>=0;i--){
            while(!stack.isEmpty()&&nums2[i]>nums2[stack.peek()]){
                stack.pop();
            }
            if(!stack.isEmpty()){
                temp[i]=stack.peek();
            }
            stack.push(i);
        }
        for(int i=0;i<nums1.length;i++){
            int idx=temp[map.get(nums1[i])];
            if(idx==0){
                ans[i]=-1;
            }else{
                ans[i]=nums2[idx];
            }
        }
        return ans;
    }
}
```
### 解法二
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        if(nums1==null||nums1.length==0){
            return new int[0];
        }
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<nums2.length;i++){
            map.put(nums2[i],i);
        }
        int[] ans=new int[nums1.length];
        int[] temp=new int[nums2.length];
        Deque<Integer> stack=new LinkedList<>();
        for(int i=0;i<nums2.length;i++){
            while(!stack.isEmpty()&&nums2[i]>nums2[stack.peek()]){
                int preIndex=stack.pop();
                temp[preIndex]=i;
            }
            stack.push(i);
        }
        for(int i=0;i<nums1.length;i++){
            int idx=temp[map.get(nums1[i])];
            if(idx==0){
                ans[i]=-1;
            }else{
                ans[i]=nums2[idx];
            }
        }
        return ans;
    }
}
```
### 解法三
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] ans=new int[nums1.length];
        int cnt=0;
        for(int i=0;i<nums1.length;i++){
            int index=-1;
            boolean flag=false;
            for(int j=0;j<nums2.length;j++){
                if(nums2[j]==nums1[i]){
                    flag=true;
                }
                if(flag==true&&nums2[j]>nums1[i]){
                    index=nums2[j];
                    break;
                }
            }
            ans[cnt++]=index;
        }
        return ans;
    }
}
```