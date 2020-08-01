# [LeetCode219.存在重复元素II](https://leetcode-cn.com/problems/contains-duplicate-ii/)
## 题目描述
给定一个整数数组和一个整数 `k`，判断数组中是否存在两个不同的索引 `i` 和 `j`，使得 `nums [i] = nums [j]`，并且 `i` 和 `j` 的差的 绝对值 至多为 `k`。

### 示例
```
输入: nums = [1,2,3,1], k = 3
输出: true
```
```
输入: nums = [1,0,1,1], k = 1
输出: true
```
```
输入: nums = [1,2,3,1,2,3], k = 2
输出: false
```
## 题解
- 维护一个哈希表，里面始终最多包含 `k` 个元素，当出现重复值时则说明在 `k` 距离内存在重复元素
- 每次遍历一个元素则将其加入哈希表中，如果哈希表的大小大于 `k`，则移除最前面的数字
- 时间复杂度：$O(n)$，`n` 为数组长度
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        HashSet<Integer> set = new HashSet<>();
        for(int i = 0; i < nums.length; i++) {
            if(set.contains(nums[i])) {
                return true;
            }
            set.add(nums[i]);
            if(set.size() > k) {
                set.remove(nums[i - k]);
            }
        }
        return false;
    }
}
```
自己的解法:
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.containsKey(nums[i])){
                int idx=map.get(nums[i]);
                if(Math.abs(idx-i)<=k){
                    return true;
                }
            }
            map.put(nums[i],i);
        }
        return false;
    }
}
```