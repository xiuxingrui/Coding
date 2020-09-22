# [LeetCode416.分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)
## 题目描述
给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意:

1. 每个数组中的元素不会超过 100
2. 数组的大小不会超过 200

### 示例
```
输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
```
```
输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.
```
## 题解
### 解法一
那么对于这个问题，我们可以先对集合求和，得出 `sum`，把问题转化为背包问题：

给一个可装载重量为 `sum / 2` 的背包和 `N` 个物品，每个物品的重量为 `nums[i]`。现在让你装物品，是否存在一种装法，能够恰好将背包装满？

`dp[i][j] = x` 表示，对于前 `i` 个物品，当前背包的容量为 `j` 时，若 `x` 为 `true`，则说明可以恰好将背包装满，若 `x` 为 `false`，则说明不能恰好将背包装满。

如果不把 `nums[i]` 算入子集，或者说你不把这第 `i` 个物品装入背包，那么是否能够恰好装满背包，取决于上一个状态 `dp[i-1][j]`，继承之前的结果。

如果把 `nums[i]` 算入子集，或者说你把这第 `i` 个物品装入了背包，那么是否能够恰好装满背包，取决于状态 `dp[i - 1][j-nums[i-1]]`。


```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums==null||nums.length==0){
            return false;
        }
        int sum=0;
        for(int num:nums){
            sum+=num;
        }
        if(sum%2==1){
            return false;
        }
        boolean[][] dp=new boolean[nums.length+1][sum/2+1];
        for(int i=0;i<=nums.length;i++){
            dp[i][0]=true;
        }
        for(int i=1;i<nums.length+1;i++){
            for(int j=1;j<=sum/2;j++){
                if(j<nums[i-1]){
                    dp[i][j]=dp[i-1][j];
                }else{
                    dp[i][j]=dp[i-1][j]||dp[i-1][j-nums[i-1]];
                }
            }
        }
        return dp[nums.length][sum/2];
    }
}
```
或
```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums==null||nums.length==0){
            return false;
        }
        int len=nums.length;
        int sum=0;
        for(int num:nums){
            sum+=num;
        }
        if(sum%2==1){
            return false;
        }
        int[][] dp=new int[len+1][sum/2+1];
        for(int i=0;i<=len;i++){
            dp[i][0]=1;
        }
        for(int i=1;i<=len;i++){
            for(int j=1;j<=sum/2;j++){
                dp[i][j]=dp[i-1][j];
                if(j>=nums[i-1]){
                    dp[i][j]+=dp[i-1][j-nums[i-1]];
                }
            }
        }
        return dp[len][sum/2]!=0;//不能写>0,因为100个100的时候dp已经越界成负数
    }
}
```
### 解法二
```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums==null||nums.length==0){
            return false;
        }
        int sum=0;
        for(int num:nums){
            sum+=num;
        }
        if(sum%2==1){
            return false;
        }
        boolean[] dp=new boolean[sum/2+1];
        dp[0]=true;
        for(int i=0;i<nums.length;i++){
            for(int j=sum/2;j>=nums[i];j--){
                dp[j]=dp[j]||dp[j-nums[i]];
            }
        }
        return dp[sum/2];
    }
}
```
或
```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums==null||nums.length==0){
            return false;
        }
        int sum=0;
        for(int num:nums){
            sum+=num;
        }
        if(sum%2==1){
            return false;
        }
        int[] dp=new int[sum/2+1];
        dp[0]=1;
        for(int i=0;i<nums.length;i++){
            for(int j=sum/2;j>=nums[i];j--){
                dp[j]+=dp[j-nums[i]];
            }
        }
        return dp[sum/2]!=0;//不能写>0,因为100个100的时候dp已经越界成负数
    }
}
```
