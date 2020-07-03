# [LeetCode268.缺失数字](https://leetcode-cn.com/problems/missing-number/)
## 题目描述
给定一个包含 0, 1, 2, ..., n 中 n 个数的序列，找出 0 .. n 中没有出现在序列中的那个数。
### 示例
```
输入: [3,0,1]
输出: 2
```
```
输入: [9,6,4,2,3,5,7,0,1]
输出: 8
```
## 题解
### 解法一：数学
用**高斯求和公式**求出 `[0..n]`的和，减去数组中所有数的和，就得到了缺失的数字。
```java
class Solution {
    public int missingNumber(int[] nums) {
        int expectedSum = nums.length*(nums.length + 1)/2;
        int actualSum = 0;
        for (int num : nums) actualSum += num;
        return expectedSum - actualSum;
    }
}
```
### 解法二:位运算
由于异或运算（XOR）满足结合律，并且对一个数进行两次完全相同的异或运算会得到原来的数，因此我们可以通过异或运算找到缺失的数字。

我们知道数组中有 `n` 个数，并且缺失的数在 `[0..n]` 中。因此我们可以先得到 `[0..n]` 的异或值，再将结果对数组中的每一个数进行一次异或运算。未缺失的数在 `[0..n]` 和数组中各出现一次，因此异或后得到 0。而缺失的数字只在 `[0..n]` 中出现了一次，在数组中没有出现，因此最终的异或结果即为这个缺失的数字。

在编写代码时，由于 `[0..n]` 恰好是这个数组的下标加上 `n`，因此可以用一次循环完成所有的异或运算，例如下面这个例子：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200703171017.png)

可以将结果的初始值设为 `n`，再对数组中的每一个数以及它的下标进行一个异或运算，即：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200703171042.png)

```java
class Solution {
    public int missingNumber(int[] nums) {
        int missing = nums.length;
        for (int i = 0; i < nums.length; i++) {
            missing ^= i ^ nums[i];
        }
        return missing;
    }
}
```
### 解法三:哈希表
```java
class Solution {
    public int missingNumber(int[] nums) {
        for(int i=0;i<nums.length;++i){
            while(nums[i]!=nums.length&&nums[i]!=i){
                if(nums[i]!=nums[nums[i]]){
                    swap(nums,i,nums[i]);
                }else{
                    break;
                }
            }
        }
        for(int i=0;i<nums.length;++i){
            if(nums[i]!=i){
                return i;
            }
        }
        return nums.length;
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```


