# [LeetCode350.两个数组的交集II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)
## 题目描述
给定两个数组，编写一个函数来计算它们的交集。
- 输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
- 我们可以不考虑输出结果的顺序。

- 如果给定的数组已经排好序呢？你将如何优化你的算法？
- 如果 `nums1` 的大小比 `nums2` 小很多，哪种方法更优？
- 如果 `nums2` 的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

### 示例
```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```
```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```
## 题解
### 解法一
由于同一个数字在两个数组中都可能出现多次，因此需要用哈希表存储每个数字出现的次数。对于一个数字，其在交集中出现的次数等于该数字在两个数组中出现次数的最小值。

首先遍历第一个数组，并在哈希表中记录第一个数组中的每个数字以及对应出现的次数，然后遍历第二个数组，对于第二个数组中的每个数字，如果在哈希表中存在这个数字，则将该数字添加到答案，并减少哈希表中该数字出现的次数。

为了降低空间复杂度，首先遍历较短的数组并在哈希表中记录每个数字以及对应出现的次数，然后遍历较长的数组得到交集。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201102122040.gif)

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return intersect(nums2, nums1);
        }
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int num : nums1) {
            int count = map.getOrDefault(num, 0) + 1;
            map.put(num, count);
        }
        int[] intersection = new int[nums1.length];
        int index = 0;
        for (int num : nums2) {
            if(map.containsKey(num)&&map.get(num)>0){
                intersection[index++] = num;
                int count=map.get(num);
                count--;
                map.put(num, count);
            }
        }
        return Arrays.copyOfRange(intersection, 0, index);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(m+n)$，其中 `m` 和 `n` 分别是两个数组的长度。需要遍历两个数组并对哈希表进行操作，哈希表操作的时间复杂度是 $O(1)$，因此总时间复杂度与两个数组的长度和呈线性关系。
- 空间复杂度：$O(min(m,n))$，其中 `m` 和 `n` 分别是两个数组的长度。对较短的数组进行哈希表的操作，哈希表的大小不会超过较短的数组的长度。为返回值创建一个数组 `intersection`，其长度为较短的数组的长度。

自己的解法：
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        HashMap<Integer,Integer> map1=new HashMap<>();
        HashMap<Integer,Integer> map2=new HashMap<>();
        for(int num:nums1){
            map1.put(num,map1.getOrDefault(num,0)+1);
        }
        for(int num:nums2){
            map2.put(num,map2.getOrDefault(num,0)+1);
        }
        int[] ans=new int[Math.max(nums1.length,nums2.length)];
        int index=0;
        for(int key:map1.keySet()){
            if(map2.containsKey(key)){
                int cnt=Math.min(map1.get(key),map2.get(key));
                for(int i=0;i<cnt;i++){
                    ans[index++]=key;
                }
            }
        }
        return Arrays.copyOfRange(ans,0,index);
    }
}
```

### 解法二
如果两个数组是有序的，则可以便捷地计算两个数组的交集。

首先对两个数组进行排序，然后使用两个指针遍历两个数组。

初始时，两个指针分别指向两个数组的头部。每次比较两个指针指向的两个数组中的数字，如果两个数字不相等，则将指向较小数字的指针右移一位，如果两个数字相等，将该数字添加到答案，并将两个指针都右移一位。当至少有一个指针超出数组范围时，遍历结束。

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        List<Integer> list=new ArrayList<>();
        int i=0,j=0;
        while(i<nums1.length&&j<nums2.length){
            if(nums1[i]==nums2[j]){
                list.add(nums1[i]);
                i++;
                j++;
            }
            while(i<nums1.length&&j<nums2.length&&nums1[i]<nums2[j]){
                i++;
            }
            while(i<nums1.length&&j<nums2.length&&nums1[i]>nums2[j]){
                j++;
            }
        }
        int[] res=new int[list.size()];
        int count=0;
        for(int num:list){
            res[count++]=num;
        }
        return res;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mlogm+nlogn)$，其中 `m` 和 `n` 分别是两个数组的长度。对两个数组进行排序的时间复杂度是 $O(mlogm+nlogn)$，遍历两个数组的时间复杂度是 $O(m+n)$，因此总时间复杂度是 $O(mlogm+nlogn)$。
- 空间复杂度：$O(min(m,n))$，其中 `m` 和 `n` 分别是两个数组的长度。