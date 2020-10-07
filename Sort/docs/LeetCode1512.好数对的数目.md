# [LeetCode1512.好数对的数目](https://leetcode-cn.com/problems/number-of-good-pairs/)
## 题目描述
给你一个整数数组 `nums` 。

如果一组数字 `(i,j)` 满足 `nums[i] == nums[j]` 且 `i < j` ，就可以认为这是一组 好数对 。

返回好数对的数目。

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
### 示例
```
输入：nums = [1,2,3,1,1,3]
输出：4
解释：有 4 组好数对，分别是 (0,3), (0,4), (3,4), (2,5) ，下标从 0 开始
```
```
输入：nums = [1,1,1,1]
输出：6
解释：数组中的每组数字都是好数对
```
```
输入：nums = [1,2,3]
输出：0
```
## 题解
基数排序：
```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int[] cnt=new int[101];
        int ans=0;
        for(int num:nums){
            ans+=cnt[num];
            cnt[num]++;
        }
        return ans;
    }
}
```
暴力
```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int cnt=0;
        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                if(nums[i]==nums[j]){
                    cnt++;
                }
            }
        }
        return cnt;
    }
}
```