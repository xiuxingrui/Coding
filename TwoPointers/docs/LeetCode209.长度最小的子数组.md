# [LeetCode209.长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
## 题目描述
给定一个含有 `n` 个正整数的数组和一个正整数 `s` ，找出该数组中满足其和 `≥ s` 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。
### 示例
```
输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```
## 题解
### 解法一:前缀和+二分搜索
自己解法:
```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int len=nums.length;
        int min=len+1;
        int[] prefixSum=new int[len+1];
        prefixSum[0]=0;
        for(int i=1;i<=len;i++){
            prefixSum[i]=prefixSum[i-1]+nums[i-1];
        }
        for(int i=1;i<=len;i++){
            int left=0,right=i;
            while(left<right){
                int mid=left+(right-left)/2;
                if(prefixSum[i]-prefixSum[mid]<s){
                    right=mid;
                }else{
                    left=mid+1;
                }
            }
            if(left==0){
                continue;
            }
            else{
                min=min<(i-left+1)?min:(i-left+1);
            }
        }
        if(min==len+1){
            return 0;
        }else{
            return min;
        }
    }
}
```
或者:
```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int len=nums.length;
        int min=len+1;
        int[] prefixSum=new int[len+1];
        prefixSum[0]=0;
        for(int i=1;i<=len;i++){
            prefixSum[i]=prefixSum[i-1]+nums[i-1];
        }
        for(int i=0;i<=len;i++){
            int left=i+1,right=len+1;
            while(left<right){
                int mid=left+(right-left)/2;
                if(prefixSum[i]+s<=prefixSum[mid]){
                    right=mid;
                }else{
                    left=mid+1;
                }
            }
            if(left==len+1){
                continue;
            }
            else{
                min=min<(left-i)?min:(left-i);
            }
        }
        if(min==len+1){
            return 0;
        }else{
            return min;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogn)$，其中 `n` 是数组的长度。需要遍历每个下标作为子数组的开始下标，遍历的时间复杂度是 $O(n)$，对于每个开始下标，需要通过二分查找得到长度最小的子数组，二分查找得时间复杂度是 $O(logn)$，因此总时间复杂度是 $O(nlogn)$。

- 空间复杂度：$O(n)$，其中 `n` 是数组的长度。额外创建数组 `sums` 存储前缀和。
### 解法二：双指针
定义两个指针 `start` 和 `end` 分别表示子数组的开始位置和结束位置，维护变量 `sum` 存储子数组中的元素和（即从 `nums[start]` 到 `nums[end]` 的元素和）

初始状态下，`start` 和 `end` 都指向下标 0，`sum` 的值为 `0`。

每一轮迭代，将 `nums[end]` 加到 `sum`，如果`sum≥s`，则更新子数组的最小长度（此时子数组的长度是 `end−start+1`），然后将 `nums[start]`从 `sum` 中减去并将 `start` 右移，直到 `sum<s`，在此过程中同样更新子数组的最小长度。在每一轮迭代的最后，将 `end` 右移。


用双指针 `left` 和 `right` 表示一个窗口。

- `right` 向右移增大窗口，直到窗口内的数字和大于等于了 `s`。进行第 2 步。
- 记录此时的长度，`left` 向右移动，开始减少长度，每减少一次，就更新最小长度。直到当前窗口内的数字和小于了 `s`，回到第 1 步。

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int start = 0, end = 0;
        int sum = 0;
        while (end < n) {
            sum += nums[end];
            while (sum >= s) {
                ans = Math.min(ans, end - start + 1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是数组的长度。指针 `start` 和 `end` 最多各移动 `n` 次。

- 空间复杂度：$O(1)$。