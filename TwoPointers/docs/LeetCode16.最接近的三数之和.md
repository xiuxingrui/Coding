# [LeetCode16.最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)
## 题目描述
给定一个包括 `n` 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

提示:
- `3 <= nums.length <= 10^3`
- `-10^3 <= nums[i] <= 10^3`
- `-10^4 <= target <= 10^4`
### 示例
```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```
## 题解
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int len=nums.length;
        int res=0,min=Integer.MAX_VALUE;
        for(int i=0;i<len;++i){
            int j=i+1,k=len-1;
            while(j<k){
                int sum=nums[i]+nums[j]+nums[k];
                if(sum==target){
                    return sum;
                }
                if(Math.abs(sum-target)<min){
                    min=Math.abs(sum-target);
                    res=sum;
                }
                if((sum-target)<0){
                    while(j<k&&nums[j]==nums[j+1]) ++j;
                    ++j;
                }
                else{
                    while(j<k&&nums[k]==nums[k-1]) --k;
                    --k;
                }
            }
        }
        return res;
    }
}
```
自己的写法:
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int min=Integer.MAX_VALUE;
        int ans=0;
        for(int i=0;i<nums.length-2;i++){
            int left=i+1,right=nums.length-1;
            while(left<right){
                int sum=nums[i]+nums[left]+nums[right];
                if(sum==target){
                    return sum;
                }else if(sum<target){
                    if(target-sum<min){
                        min=target-sum;
                        ans=sum;
                    }
                    left++;
                }else{
                    if(sum-target<min){
                        min=sum-target;
                        ans=sum;
                    }
                    right--;
                }
            }
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N^2)$，其中 `N` 是数组 `nums` 的长度。我们首先需要 $O(NlogN)$ 的时间对数组进行排序，随后在枚举的过程中，使用一重循环 $O(N)$枚举 `a`，双指针 $O(N)$ 枚举 `b` 和 `c`，故一共是 $O(N^2)$。
- 空间复杂度：$O(logN)$。排序需要使用 $O(logN)$ 的空间。然而我们修改了输入的数组 `nums`，在实际情况下不一定允许，因此也可以看成使用了一个额外的数组存储了 `nums` 的副本并进行排序，此时空间复杂度为 $O(N)$。

