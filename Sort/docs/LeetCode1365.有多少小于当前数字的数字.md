# [LeetCode1365.有多少小于当前数字的数字](https://leetcode-cn.com/problems/how-many-numbers-are-smaller-than-the-current-number/)
## 题目描述
给你一个数组 `nums`，对于其中每个元素 `nums[i]`，请你统计数组中比它小的所有数字的数目。

换而言之，对于每个 `nums[i]` 你必须计算出有效的 `j` 的数量，其中 `j` 满足 `j != i` 且 `nums[j] < nums[i]` 。

以数组形式返回答案。

### 示例
```
输入：nums = [8,1,2,2,3]
输出：[4,0,1,1,3]
解释： 
对于 nums[0]=8 存在四个比它小的数字：（1，2，2 和 3）。 
对于 nums[1]=1 不存在比它小的数字。
对于 nums[2]=2 存在一个比它小的数字：（1）。 
对于 nums[3]=2 存在一个比它小的数字：（1）。 
对于 nums[4]=3 存在三个比它小的数字：（1，2 和 2）。
```
```
输入：nums = [6,5,4,8]
输出：[2,1,0,3]
```
```
输入：nums = [7,7,7,7]
输出：[0,0,0,0]
```
## 题解
### 解法一
桶排序(基数排序):
```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] cnt=new int[101];
        for(int num:nums){
            cnt[num]++;
        }
        int[] sum=new int[101];
        for(int i=1;i<=100;i++){
            sum[i]=sum[i-1]+cnt[i-1];
        }
        int[] ans=new int[nums.length];
        for(int i=0;i<nums.length;i++){
            ans[i]=sum[nums[i]];
        }
        return ans;
    }
}
```
### 解法二
排序+哈希表：
```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] ans=new int[nums.length];
        HashMap<Integer,List<Integer>> map=new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.containsKey(nums[i])){
                map.get(nums[i]).add(i);
            }else{
                List<Integer> temp=new ArrayList<>();
                temp.add(i);
                map.put(nums[i],temp);
            }
        }
        Arrays.sort(nums);
        int i=0;
        while(i<nums.length){
            List<Integer> list=map.get(nums[i]);
            for(int k:list){
                ans[k]=i;
            }
            int j=i;
            while(j<nums.length&&nums[j]==nums[i]){
                j++;
            }
            i=j;
        }
        return ans;
    }
}
```
### 解法三
暴力
```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] ans=new int[nums.length];
        for(int i=0;i<nums.length;i++){
            int cnt=0;
            for(int j=0;j<nums.length;j++){
                if(nums[j]<nums[i]){
                    cnt++;
                }
            }
            ans[i]=cnt;
        }
        return ans;
    }
}
```

