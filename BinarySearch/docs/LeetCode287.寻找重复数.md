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
### 解法一:二分法
注意：这个方法使用的前提是，题目中说：

给定一个包含 `n+1` 个整数的数组 `nums`，其数字都在 1 到 `n` 之间（包括 1 和 `n`）。

如果测试数据不在这个范围里，二分法失效。

- 通过这个方法知道二分法还可以用于确定一个有范围的整数（这个思路很常见）；
- 本题的场景和限制是极其特殊的，实际工作中和绝大多数算法问题都不会用「时间换空间」。

容易想到的方法有：

- 使用哈希表判重，这违反了限制 2；
- 将原始数组排序，排序以后，重复的数相邻，即找到了重复数，这违反了限制 1；
- 当两个数发现要放在同一个地方的时候，就发现了这个重复的元素，这违反了限制 1；
- 既然要定位数，这个数恰好是一个整数，可以在「整数的有效范围内」做二分查找，但是比较烦的一点是得反复看整个数组好几次，本题解就介绍通过二分法定位一个有范围的整数；

- 这道题要求我们查找的数是一个整数，并且给出了这个整数的范围（在 `1` 和 `n` 之间，包括 `1` 和 `n`），并且给出了一些限制，于是可以使用二分查找法定位在一个区间里的整数；
- 二分法的思路是先猜一个数（有效范围 `[left, right]`里的中间数 `mid`），然后统计原始数组中小于等于这个中间数的元素的个数 `cnt`，如果 `cnt` 严格大于 `mid`，（注意我加了着重号的部分「小于等于」、「严格大于」）。根据抽屉原理，重复元素就在区间 `[left, mid]` 里；
- 与绝大多数二分法问题的不同点是：正着思考是容易的，即：思考哪边区间存在重复数是容易的，因为有抽屉原理做保证。我们通过一个具体的例子来分析应该如何编写代码；

以 `[2, 4, 5, 2, 3, 1, 6, 7]` 为例，一共 `8` 个数，`n + 1 = 8`，`n = 7`，根据题目意思，每个数都在 `1` 和 `7` 之间。

例如：区间 `[1,7]` 的中位数是 `4`，遍历整个数组，统计小于等于 `4` 的整数的个数，如果不存在重复元素，最多为 `4` 个。等于 `4` 的时候区间 `[1,4]` 内也可能有重复元素。但是，如果整个数组里小于等于 `4` 的整数的个数严格大于 `4` 的时候，就可以说明重复的数存在于区间 `[1,4]`。

说明：由于这个算法是空间敏感的，「用时间换空间」是反常规做法，算法的运行效率肯定不会高。

```java
public class Solution {

    public int findDuplicate(int[] nums) {
        int len = nums.length;
        int left = 1;
        int right = len - 1;
        while (left < right) {
            int mid = left+(right-left)/2;
            
            int cnt = 0;
            for (int num : nums) {
                if (num <= mid) {
                    cnt += 1;
                }
            }

            // 根据抽屉原理，小于等于 4 的个数如果严格大于 4 个
            // 此时重复元素一定出现在 [1, 4] 区间里
            if (cnt > mid) {
                // 重复元素位于区间 [left, mid]
                right = mid;
            } else {
                // if 分析正确了以后，else 搜索的区间就是 if 的反面
                // [mid + 1, right]
                left = mid + 1;
            }
        }
        return left;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(NlogN)$，二分法的时间复杂度为 $O(logN)$，在二分法的内部，执行了一次 `for` 循环，时间复杂度为 $O(N)$，故时间复杂度为$O(NlogN)$。
- 空间复杂度：$O(1)$，使用了一个 `cnt` 变量，因此空间复杂度为 $O(1)$。

### 解法二:哈希表
```java
public int findDuplicate(int[] nums) {
    HashSet<Integer> set = new HashSet<>();
    for (int i = 0; i < nums.length; i++) {
        if (set.contains(nums[i])) {
            return nums[i];
        }
        set.add(nums[i]);
    }
    return -1;
}
```
原地Hash:

写法一:
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int len=nums.length;
        for(int i=0;i<len;i++){
            if(nums[Math.abs(nums[i])]<0){
                return Math.abs(nums[i]);
            }else{
                nums[Math.abs(nums[i])]*=-1;
            }
        }
        return -1;
    }
}
```
写法二：
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
写法三:
```java
class Solution {
    public int findDuplicate(int[] nums) {
        for(int i=1;i<nums.length;i++){
            while(nums[i]!=i){
                if(nums[i]!=nums[nums[i]]){
                    swap(nums,i,nums[i]);
                }
                else{
                    return nums[i];
                }
            }
        }
        return nums[0];
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
### 解法三
排序:
```java
public int findDuplicate(int[] nums) {
    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 1; i++) {
        if (nums[i] == nums[i + 1]) {
            return nums[i];
        }
    }
    return -1;
}
```


