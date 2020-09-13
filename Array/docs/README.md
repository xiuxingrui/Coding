# [LeetCode581.最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)
## 题目描述
给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是最短的，请输出它的长度。

- 输入的数组长度范围在 `[1, 10,000]`。
- 输入的数组可能包含重复元素 ，所以升序的意思是`<=`。
### 示例
```
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```
## 题解
### 解法一
#### 复杂度分析
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
### 解法二
另一个简单的想法是：我们将数组 `nums` 进行排序，记为 `nums_sorted`。然后我们比较 `nums` 和 `nums_sorted` 的元素来决定最左边和最右边不匹配的元素。它们之间的子数组就是要求的最短无序子数组。
```java
public class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] snums = nums.clone();
        Arrays.sort(snums);
        int start = snums.length, end = -1;
        for (int i = 0; i < snums.length; i++) {
            if (snums[i] != nums[i]) {
                start = Math.min(start, i);
                end = Math.max(end, i);
            }
        }
        return (end - start >= 0 ? end - start + 1 : 0);
    }
}
```
或
```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int len=nums.length;
        if(len==0||len==1){
            return 0;
        }
        int[] temp=new int[len];
        temp=Arrays.copyOf(nums,len);
        Arrays.sort(temp);
        int left=-1,right=-1;
        for(int i=0;i<len;i++){
            if(nums[i]!=temp[i]){
                left=i;
                break;
            }
        }
        for(int i=len-1;i>=0;i--){
            if(nums[i]!=temp[i]){
                right=i;
                break;
            }
        }
        if(left==-1){
            return 0;
        }else{
            return right-left+1;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogn)$。排序消耗 `nlogn` 的时间。
- 空间复杂度：$O(n)$ 。我们拷贝了一份原数组来进行排序。
