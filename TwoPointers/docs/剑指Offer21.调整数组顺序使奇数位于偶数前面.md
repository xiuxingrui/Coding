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
### 解法一
快慢双指针

- 定义快慢双指针 `i` 和 `j` ，`i` 在前， `j` 在后 .
- `i` 的作用是向前搜索奇数位置，`j` 的作用是指向下一个奇数应当存放的位置
- `i` 向前移动，当它搜索到奇数时，将它和 `nums[j]` 交换，此时 `j` 向前移动一个位置 .
- 重复上述操作，直到 `i` 指向数组末尾 .

```java
class Solution {
    public int[] exchange(int[] nums) {
        int j=0;
        for(int i=0;i<nums.length;i++){
            if(nums[i]%2==1){
                if(i!=j){
                    int temp=nums[j];
                    nums[j]=nums[i];
                    nums[i]=temp;
                }
                j++;
            }
        }
        return nums;
    }
}
```
### 解法二
考虑定义双指针 `i` , `j` 分列数组左右两端，循环执行：
- 指针 `i` 从左向右寻找偶数；
- 指针 `j` 从右向左寻找奇数；
- 将 偶数 `nums[i]` 和 奇数 `nums[j]` 交换。
可始终保证： 指针 `i` 左边都是奇数，指针 `j` 右边都是偶数 。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20210116203558.png)

算法流程：
- 初始化： `i` , `j` 双指针，分别指向数组 `nums` 左右两端；
- 循环交换： 当 `i = j` 时跳出；
  - 指针 `i` 遇到奇数则执行 `i=i+1` 跳过，直到找到偶数；
  - 指针 `j` 遇到偶数则执行 `j=j−1` 跳过，直到找到奇数；
- 交换 `nums[i]` 和 `nums[j]` 值；
返回值： 返回已修改的 `nums` 数组。

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
#### 复杂度分析
- 时间复杂度 $O(N)$ ： `N` 为数组 `nums` 长度，双指针 `i`, `j` 共同遍历整个数组。
- 空间复杂度 $O(1)$： 双指针 `i`, `j` 使用常数大小的额外空间。

