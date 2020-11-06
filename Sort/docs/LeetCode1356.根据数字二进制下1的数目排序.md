# [LeetCode1356.根据数字二进制下1的数目排序](https://leetcode-cn.com/problems/sort-integers-by-the-number-of-1-bits/)
## 题目描述
给你一个整数数组 `arr` 。请你将数组中的元素按照其二进制表示中数字 1 的数目升序排序。

如果存在多个数字二进制中 1 的数目相同，则必须将它们按照数值大小升序排列。

请你返回排序后的数组。

- `1 <= arr.length <= 500`
- `0 <= arr[i] <= 10^4`
### 示例
```
输入：arr = [0,1,2,3,4,5,6,7,8]
输出：[0,1,2,4,8,3,5,6,7]
解释：[0] 是唯一一个有 0 个 1 的数。
[1,2,4,8] 都有 1 个 1 。
[3,5,6] 有 2 个 1 。
[7] 有 3 个 1 。
按照 1 的个数排序得到的结果数组为 [0,1,2,4,8,3,5,6,7]
```
```
输入：arr = [1024,512,256,128,64,32,16,8,4,2,1]
输出：[1,2,4,8,16,32,64,128,256,512,1024]
解释：数组中所有整数二进制下都只有 1 个 1 ，所以你需要按照数值大小将它们排序。
```
```
输入：arr = [10000,10000]
输出：[10000,10000]
```
```
输入：arr = [2,3,5,7,11,13,17,19]
输出：[2,3,5,17,7,11,13,19]
```
```
输入：arr = [10,100,1000,10000]
输出：[10,100,10000,1000]
```
## 题解
```java
class Solution {
    public int[] sortByBits(int[] arr) {
        Integer[] nums = new Integer[arr.length];
        for (int i = 0; i < arr.length; i++) {
            nums[i] = arr[i];
        }
        Arrays.sort(nums, (o1, o2) -> {
            int bitCountA = Integer.bitCount(o1);
            int bitCountB = Integer.bitCount(o2);
            // 相同按原数，不同按位数
            return bitCountA == bitCountB ? o1 - o2 : bitCountA - bitCountB;
        });
        for (int i = 0; i < arr.length; i++) {
            arr[i] = nums[i];
        }
        return arr;
    }
}
```
或
```java
class Solution {
    public int[] sortByBits(int[] arr) {
        Integer[] sortArr = new Integer[arr.length];
        for(int i = 0; i < arr.length; i++) {
            sortArr[i] = arr[i];
        }
        Arrays.sort(sortArr, new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                int cntA=cntOne(a),cntB=cntOne(b);
                if(cntA!=cntB){
                    return cntA-cntB;
                }else{
                    return a-b;
                }
            }
        });
        for(int i = 0; i < arr.length; i++) {
            arr[i] = sortArr[i];
        }
        return arr;
    }
    public int cntOne(int num){
        int cnt=0;
        while(num!=0){
            cnt+=num&1;
            num>>=1;
        }
        return cnt;
    }
}

```
优化：
```java
class Solution {
    public int[] sortByBits(int[] arr) {
        Integer[] nums = new Integer[arr.length];
        for (int i = 0; i < arr.length; i++) {
            nums[i] = arr[i];
        }
        Arrays.sort(nums,new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                int cntA=cntOne(a),cntB=cntOne(b);
                if(cntA!=cntB){
                    return cntA-cntB;
                }else{
                    return a-b;
                }
            }
        });
        for (int i = 0; i < arr.length; i++) {
            arr[i] = nums[i];
        }
        return arr;
    }
    public int cntOne(int num){
        int count = 0;
        while(num!=0){
            num&= (num -1);
            count++;
        }
        return count;
    }
}
```
简单解释一下就是，对非零整数 `n`， `n-1` 操作必然会破坏最后一个 1，导致最后一个 1 变为 0，其后的值变为 1。而对于原值 `n`，最后一个 1 的位置为 1，后面的值为 0，恰好与 `n-1` 相反，并操作后即全部抵消为 0。而最后一个 1 前面的内容保持不变，恰好就去掉了最后的 1。

这种解法下，比如一个数原本是 10011101，下次循环后变为 10011100，再下次变为 10011000，此后依次为 10010000，10000000，00000000。

自己的解法：
```java
class Solution {
    public int[] sortByBits(int[] arr) {
        Integer[] nums = new Integer[arr.length];
        for (int i = 0; i < arr.length; i++) {
            nums[i] = arr[i];
        }
        Arrays.sort(nums,new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                int cntA=cntOne(a),cntB=cntOne(b);
                if(cntA!=cntB){
                    return cntA-cntB;
                }else{
                    return a-b;
                }
            }
        });
        for (int i = 0; i < arr.length; i++) {
            arr[i] = nums[i];
        }
        return arr;
    }
    public int cntOne(int num){
        int cnt=0;
        while(num!=0){
            cnt+=num%2;
            num/=2;
        }
        return cnt;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(nlogn)$，其中 `n` 为整数数组 `arr` 的长度。
- 空间复杂度：$O(n)$，其中 `n` 为整数数组 `arr` 的长度。