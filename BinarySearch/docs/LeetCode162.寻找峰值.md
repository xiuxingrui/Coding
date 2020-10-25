# [LeetCode162.寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)
## 题目描述
峰值元素是指其值大于左右相邻值的元素。

给定一个输入数组 `nums`，其中 `nums[i] ≠ nums[i+1]`，找到峰值元素并返回其索引。

数组可能包含多个峰值，在这种情况下，返回任何一个峰值所在位置即可。

你可以假设 `nums[-1] = nums[n] = -∞`。

你的解法应该是 $O(logN)$ 时间复杂度的。
### 示例
```
输入: nums = [1,2,3,1]
输出: 2
解释: 3 是峰值元素，你的函数应该返回其索引 2。
```
```
输入: nums = [1,2,1,3,5,6,4]
输出: 1 或 5 
解释: 你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
```
## 题解
### 解法一

首先要注意题目条件，在题目描述中出现了 `nums[-1] = nums[n] = -∞`，这就代表着 只要数组中存在一个元素比相邻元素大，那么沿着它一定可以找到一个峰值。

根据左右指针计算中间位置 `m`，并比较 `m` 与 `m+1` 的值，如果 `m` 较大，则左侧存在峰值，`r = m`，如果 `m + 1` 较大，则右侧存在峰值，`l = m + 1`

这个题为什么可以用二分呢？

不管什么情况，之所以能用二分，是因为我们可以根据某个条件，直接抛弃一半的元素，从而使得时间复杂度降到 `log` 级别。

至于这道题，因为题目告诉我们可以返回数组中的任意一个峰顶。所以我们只要确定某一半至少存在一个峰顶，那么另一半就可以抛弃掉。

我们只需要把 `nums[mid]` 和 `nums[mid + 1]` 比较。

先考虑第一次二分的时候，`start = 0`，`end = nums.length - 1`。

如果 `nums[mid] < nums[mid + 1]`，此时在上升阶段，因为 `nums[n]` 看做负无穷，也就是最终一定会下降，所以 `mid + 1` 到 `end` 之间至少会存在一个峰顶，可以把左半部分抛弃。

如果 `nums[mid] > nums[mid + 1]`，此时在下降阶段，因为 `nums[0]` 看做负无穷，最初一定是上升阶段，所以 `start` 到 `mid` 之间至少会存在一个峰顶，可以把右半部分抛弃。

通过上边的切割，我们就保证了后续左边界一定是在上升，右边界一定是在下降，所以第二次、第三次... 的二分就和上边一个道理了。

```java
class Solution {
    public int findPeakElement(int[] nums) {
        if(nums.length==1){
            return 0;
        }
        int i=0,j=nums.length-1;
        while(i<=j){
            int mid=i+(j-i)/2;
            if(mid==0&&nums[0]>nums[1]||mid==nums.length-1&&nums[mid]>nums[mid-1]||mid>0&&mid<nums.length-1&&nums[mid-1]<nums[mid]&&nums[mid]>nums[mid+1]){
                return mid;
            }else if(mid==0||mid>0&&nums[mid-1]<nums[mid]){
                i=mid+1;
            }else{
                j=mid-1;
            }
        }
        return -1;
    }
}
```
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int i=0,j=nums.length-1;
        while(i<j){
            int mid=i+(j-i)/2;
            //找到第一个逆序对，即满足要求
            //如果nums[i] > nums[i+1]，则在i之前一定存在峰值元素
            if(nums[mid]>nums[mid+1]){
                j=mid;
            }else{//如果nums[i] < nums[i+1]，则在i+1之后一定存在峰值元素
                i=mid+1;
            }
        }
        return i;
    }
}
```
#### 复杂度分析
- 时间复杂度 : $O(log2(n))$。 每一步都将搜索空间减半。其中 `n` 为 `nums` 数组的长度。
- 空间复杂度 : $O(1)$。 只使用了常数空间。

### 解法二
本方法利用了连续的两个元素 `nums[j]` 和 `nums[j+1]` 不会相等这一事实。于是，我们可以从头开始遍历 `nums` 数组。每当我们遇到数字 `nums[i]`，只需要检查它是否大于下一个元素 `nums[i+1]` 即可判断 `nums[i]` 是否是峰值。可以通过分别讨论问题的全部三种可能情况来理解本方法的思路。

情况 1. 所有的数字以降序排列。这种情况下，第一个元素即为峰值。我们首先检查当前元素是否大于下个元素。第一个元素满足这一条件，因此被正确判断为峰值。此时，我们不需要继续向下判断，也就不会有需要判断 `nums[i]` 和上一个元素 `nums[i−1]` 的大小的情况。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201025162644.png)

情况 2. 所有的数字以升序排列。这种情况下，我们会一直比较 `nums[i]` 与 `nums[i+1]` 以判断 `nums[i]` 是否是峰值元素。没有元素符合这一条件，说明处于上坡而非峰值。于是，在结尾，我们返回末尾元素作为峰值元素，得到正确结果。在这种情况下，我们同样不需要比较 `nums[i]` 和上一个元素 `nums[i−1]`，因为处于上坡是 `nums[i]` 不是峰值的充分条件。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201025163737.png)

情况 3. 峰值出现在中间某处。这种情况下，当遍历上升部分时，与情况 2 相同，没有元素满足 `nums[i]>nums[i+1]`。我们不需要比较 `nums[i]` 和上一个元素 `nums[i−1]`。当到达峰值元素时，`nums[i]>nums[i+1]` 条件满足。此时，我们同样不需要比较 `nums[i]` 和上一个元素 `nums[i−1]`。这是由于“遍历会到达第i个元素”本身就说明上一个元素（第`i-1`个）不满足 `nums[i]>nums[i+1]` 这一条件，也就说明 `nums[i−1]<nums[i]`。于是，我们同样可以得到正确结果。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201025163854.png)

```java
public class Solution {
    public int findPeakElement(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] > nums[i + 1])
                return i;
        }
        return nums.length - 1;
    }
}
```
自己的写法：
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int len=nums.length;
        if(len==1){
            return 0;
        }
        for(int i=1;i<len-1;i++){
            if(nums[i-1]<nums[i]&&nums[i]>nums[i+1]){
                return i;
            }
        }
        if(nums[0]>=nums[len-1]){
            return 0;
        }else{
            return len-1;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度 : $O(n)$。 我们对长度为 `n` 的数组 `nums` 只进行一次遍历。
- 空间复杂度 : $O(1)$。 只使用了常数空间。
