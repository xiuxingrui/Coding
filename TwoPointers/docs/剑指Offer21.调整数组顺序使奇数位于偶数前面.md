# [剑指Offer21.调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)
## 题目描述
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

### 示例
```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```
## 题解
```java
class Solution {
    public int[] exchange(int[] nums) {
        int i=0,j=nums.length-1;
        while(i<j){
            while(nums[i]%2!=0&&i<j){
                ++i;
            }
            while(nums[j]%2==0&&i<j){
                --j;
            }
            //此处不用判断i是否小于j
            int temp=nums[i];
            nums[i]=nums[j];
            nums[j]=temp;
        }
        return nums;
    }
}
```