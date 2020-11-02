# [LeetCode349.两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)
## 题目描述
给定两个数组，编写一个函数来计算它们的交集。

### 示例
```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```
```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
```
## 题解
### 解法一
自己的解法
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> temp=new HashSet<>();
        Set<Integer> set=new HashSet<>();
        for(int num:nums1){
            set.add(num);
        }
        for(int num:nums2){
            if(set.contains(num)){
                temp.add(num);
            }
        }
        int len=temp.size();
        int[] res=new int[len];
        int count=0;
        for(int num:temp){
            res[count++]=num;
        }
        return res;
    }
}
```
或：

首先使用两个集合分别存储两个数组中的元素，然后遍历较小的集合，判断其中的每个元素是否在另一个集合中，如果元素也在另一个集合中，则将该元素添加到返回值。该方法的时间复杂度可以降低到 $O(m+n)$。

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<Integer>();
        Set<Integer> set2 = new HashSet<Integer>();
        for (int num : nums1) {
            set1.add(num);
        }
        for (int num : nums2) {
            set2.add(num);
        }
        return getIntersection(set1, set2);
    }

    public int[] getIntersection(Set<Integer> set1, Set<Integer> set2) {
        if (set1.size() > set2.size()) {
            return getIntersection(set2, set1);
        }
        Set<Integer> intersectionSet = new HashSet<Integer>();
        for (int num : set1) {
            if (set2.contains(num)) {
                intersectionSet.add(num);
            }
        }
        int[] intersection = new int[intersectionSet.size()];
        int index = 0;
        for (int num : intersectionSet) {
            intersection[index++] = num;
        }
        return intersection;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(m+n)$，其中 `m` 和 `n` 分别是两个数组的长度。使用两个集合分别存储两个数组中的元素需要 $O(m+n)$ 的时间，遍历较小的集合并判断元素是否在另一个集合中需要 $O(min(m,n))$ 的时间，因此总时间复杂度是 $O(m+n)$。
- 空间复杂度：$O(m+n)$，其中 `m` 和 `n` 分别是两个数组的长度。空间复杂度主要取决于两个集合。

### 解法二
如果两个数组是有序的，则可以使用双指针的方法得到两个数组的交集。

首先对两个数组进行排序，然后使用两个指针遍历两个数组。可以预见的是加入答案的数组的元素一定是递增的，为了保证加入元素的唯一性，我们需要额外记录变量 `pre` 表示上一次加入答案数组的元素。

初始时，两个指针分别指向两个数组的头部。每次比较两个指针指向的两个数组中的数字，如果两个数字不相等，则将指向较小数字的指针右移一位，如果两个数字相等，且该数字不等于 `pre` ，将该数字添加到答案并更新 `pre` 变量，同时将两个指针都右移一位。当至少有一个指针超出数组范围时，遍历结束。

```java
public int[] intersection(int[] nums1, int[] nums2) {
  Set<Integer> set = new HashSet<>();
  Arrays.sort(nums1);
  Arrays.sort(nums2);
  int i = 0, j = 0;
  while (i < nums1.length && j < nums2.length) {
    if (nums1[i] == nums2[j]) {
      set.add(nums1[i]);
      i++;
      j++;
    } else if (nums1[i] < nums2[j]) {
      i++;
    } else if (nums1[i] > nums2[j]) {
      j++;
    }
  }
  int[] res = new int[set.size()];
  int index = 0;
  for (int num : set) {
    res[index++] = num;
  }
  return res;
}
```
#### 复杂度分析
- 时间复杂度：$O(mlogm+nlogn)$，其中 `m` 和 `n` 分别是两个数组的长度。对两个数组排序的时间复杂度分别是 $O(mlogm)$ 和 $O(nlogn)$，双指针寻找交集元素的时间复杂度是 $O(m+n)$，因此总时间复杂度是 $O(mlogm+nlogn)$。
- 空间复杂度：$O(logm+logn)$，其中 `m` 和 `n` 分别是两个数组的长度。空间复杂度主要取决于排序使用的额外空间。