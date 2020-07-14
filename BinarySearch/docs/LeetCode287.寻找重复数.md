# [LeetCode287.寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)
## 题目描述
给定一个包含 `n + 1` 个整数的数组 `nums`，其数字都在 `1` 到 `n` 之间（包括 `1` 和 `n`），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

### 示例
```
输入: [1,3,4,2,2]
输出: 2

输入: [3,1,3,4,2]
输出: 3
```
### 说明

- **不能**更改原数组（假设数组是只读的）。
- 只能使用额外的 $O(1)$ 的空间。
- 时间复杂度小于 $O(n^2)$ 。
- 数组中只有一个重复的数字，但它可能不止重复出现一次。

## 题解

### 解法三：原地哈希
不符合题目要求，但是面试中可能不会有那么多限制，随手记录一下。
分成两部分，`nums[0]~nums[nums.length-2]`和`nums[nums.length-1]`，重复的数如果不在前半部分就在后半部分。
```java
class Solution {
    public int findDuplicate(int[] nums) {
        for(int i=0;i<nums.length-1;++i){
            while(nums[i]!=i+1){
                if(nums[i]!=nums[nums[i]-1]){
                    swap(nums,i,nums[i]-1);
                }
                else{
                    return nums[i];
                }
            }
        }
        return nums[nums.length-1];
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
改进:(性能较差改进前差)
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int res=0;
        for(int i=0;i<nums.length;++i){
            if(nums[Math.abs(nums[i])]<0){
                res=Math.abs(nums[i]);
            }else{
                nums[Math.abs(nums[i])]*=-1;
            }
        }
        return res;
    }
}
```

