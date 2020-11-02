# [LeetCode1213.三个有序数组的交集](https://leetcode-cn.com/problems/intersection-of-three-sorted-arrays/)
## 题目描述
给出三个均为 严格递增排列 的整数数组 `arr1`，`arr2` 和 `arr3`。

返回一个由 仅 在这三个数组中 同时出现 的整数所构成的有序数组。

- `1 <= arr1.length, arr2.length, arr3.length <= 1000`
- `1 <= arr1[i], arr2[i], arr3[i] <= 2000`

### 示例
```
输入: arr1 = [1,2,3,4,5], arr2 = [1,2,5,7,9], arr3 = [1,3,4,5,8]
输出: [1,5]
解释: 只有 1 和 5 同时在这三个数组中出现.
```
## 题解
### 解法一
```java
class Solution {
    public List<Integer> arraysIntersection(int[] arr1, int[] arr2, int[] arr3) {
        List<Integer> nums = new LinkedList<>();
        for(int i=0,j=0,k=0;i<arr1.length&&j<arr2.length&&k<arr3.length;i++,j++,k++){
            int a = arr1[i];
            int b = arr2[j];
            int c = arr3[k];
            if(a==b&&b==c){
                nums.add(a);
            }
            if(a>b||a>c){
                i--;
            }
            if(b>a||b>c){
                j--;
            }
            if(c>a||c>b){
                k--;
            }
        }
        return nums;
    }
}
```
### 解法二
```java
class Solution {
    public List<Integer> arraysIntersection(int[] arr1, int[] arr2, int[] arr3) {
        Set<Integer> set1=new HashSet<>();
        Set<Integer> set2=new HashSet<>();
        List<Integer> res=new ArrayList<>();
        for(int num:arr1){
            set1.add(num);
        }
        for(int num:arr2){
            if(set1.contains(num)){
                set2.add(num);
            }
        }
        for(int num:arr3){
            if(set2.contains(num)){
                res.add(num);
            }
        }
        return res;
    }
}
```