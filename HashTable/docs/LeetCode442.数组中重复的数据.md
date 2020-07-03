# [LeetCode442.数组中重复的数据](https://leetcode-cn.com/problems/find-all-duplicates-in-an-array/)
## 题目描述
给定一个整数数组 `a`，其中`1 ≤ a[i] ≤ n` （`n`为数组长度）, 其中有些元素出现两次而其他元素出现一次。

找到所有出现两次的元素。

你可以不用到任何额外空间并在$O(n)$时间复杂度内解决这个问题吗？

### 示例
```
输入:
[4,3,2,7,8,2,3,1]

输出:
[2,3]
```
## 题解
### 解法一
```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> result = new LinkedList<Integer>();
        for (int i = 0; i < nums.length; i++) {
            int newIndex = Math.abs(nums[i]) - 1;
            if (nums[newIndex] > 0) {
                nums[newIndex] *= -1;
            }else {
                result.add(Math.abs(nums[i]));
            }
        }
        return result;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$。
- 空间复杂度：$O(1)$，因为我们在原地修改数组，没有使用额外的空间。
### 解法二
```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> list=new ArrayList<>();
        for(int i=0;i<nums.length;++i){
            while(nums[i]!=i+1){
                if(nums[nums[i]-1]==nums[i]){
                    break;
                }else{
                    swap(nums,i,nums[i]-1);
                }
            }
        }
        for(int i=0;i<nums.length;++i){
            if(nums[i]!=i+1)
                list.add(nums[i]);
        }
        return list;
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index2];
        nums[index2]=nums[index1];
        nums[index1]=temp;
    }
}
```