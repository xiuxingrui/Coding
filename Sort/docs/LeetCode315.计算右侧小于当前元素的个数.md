# [LeetCode315.计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)
## 题目描述
给定一个整数数组 `nums`，按要求返回一个新数组 `counts`。数组 `counts` 有该性质： `counts[i]` 的值是  `nums[i]` 右侧小于 `nums[i]` 的元素的数量。

### 示例
```java
输入：[5,2,6,1]
输出：[2,1,1,0] 
解释：
5 的右侧有 2 个更小的元素 (2 和 1)
2 的右侧仅有 1 个更小的元素 (1)
6 的右侧有 1 个更小的元素 (1)
1 的右侧有 0 个更小的元素
```
## 题解
使用「归并排序」求解这道问题，需要有求解「逆序对」的经验。

1. 求解「逆序对」的思想：当其中一个数字放进最终归并以后的有序数组中的时候，这个数字与之前看过的数字个数（或者是未看过的数字个数）可以直接统计出来，而不必一个一个数。这样排序完成以后，原数组的逆序数也就计算出来了；
2. 具体来说，本题让我们求「在一个数组的某个元素的右边，比自己小的元素的个数」，因此，需要在「前有序数组」的元素归并的时候，数一数「后有序数组」已经归并回去的元素的个数，因为这些已经出列的元素都比当前出列的元素要（严格）小；
3. 但是在「归并」的过程中，元素的位置会发生变化，因此下一步需要思考如何定位元素；根据「索引堆」的学习经验，一个元素在算法的执行过程中位置发生变化，我们还想定位它，可以使用「索引数组」，技巧在于：「原始数组」不变，用于比较两个元素的大小，真正位置变化的是「索引数组」的位置；
4. 索引数组」技巧建立了一个一一对应的关系，记录了当前操作的数对应的「原始数组」的下标
5. 「归并排序」还需要一个用于归并的辅助数组，这个时候拷贝的就是索引数组的值了。
```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> list=new ArrayList<>();
        int[] indexes=new int[nums.length];
        int[] res=new int[nums.length];
        for(int i=0;i<nums.length;i++){
            indexes[i]=i;
        }
        countPairs(nums,0,nums.length-1,indexes,res);
        for(int i=0;i<nums.length;i++){
            list.add(res[i]);
        }
        return list;
    }
    public void countPairs(int[] nums,int start,int end,int[] indexes,int[] res){
        if(start>=end){
            return;
        }
        int mid=start+(end-start)/2;
        countPairs(nums,start,mid,indexes,res);
        countPairs(nums,mid+1,end,indexes,res);
        if(nums[indexes[mid]] <= nums[indexes[mid + 1]]){
            return;
        }
        merge(nums,start,mid,end,indexes,res);
    }
    public void merge(int[] nums,int start,int mid,int end,int[] indexes,int[] res){
        int left=start,right=mid+1;
        int[] temp=new int[end-start+1];
        int index=0;
        while(left<=mid&&right<=end){
            if(nums[indexes[left]]<=nums[indexes[right]]){
                temp[index++]=indexes[left];
                res[indexes[left]]+=right-mid-1;
                left++;
            }else{
                temp[index++]=indexes[right];
                right++;
            }
        }
        while(left<=mid){
            temp[index++]=indexes[left];
            res[indexes[left]]+=end-mid;
            left++;
        }
        while(right<=end){
            temp[index++]=indexes[right++];
        }
        for(int i=0;i<index;i++){
            indexes[start+i]=temp[i];
        }
    }
}
```
### 复杂度分析
- 时间复杂度：$O(NlogN)$，数组的元素个数是 `N`，递归执行分治法，时间复杂度是对数级别的，因此时间复杂度是 $O(NlogN)$。
- 空间复杂度：$O(N)$，需要 3 个数组，一个索引数组，一个临时数组用于索引数组的归并，还有一个结果数组，它们的长度都是 `N`，故空间复杂度是 $O(N)$。