# [LeetCode215.数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)
## 题目描述
在未排序的数组中找到第 `k` 个最大的元素。请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。


你可以假设 `k` 总是有效的，且 `1≤k≤数组的长度`。
### 示例
```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```
```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```
## 题解
### 解法二:
建堆:
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int len=nums.length;
        heapify(nums);
        int ans=0;
        for(int i=0;i<k;i++){
            ans=nums[0];
            swap(nums,0,len-1-i);
            siftDown(nums,0,len-2-i);
        }
        return ans;
    }
    public void heapify(int[] nums){
        for(int i=(nums.length-1)/2;i>=0;i--){
            siftDown(nums,i,nums.length-1);
        }
    }
    public void siftDown(int[] nums,int start,int end){
        int i=start,j=2*i+1;
        while(j<=end){
            if(j+1<=end&&nums[j]<nums[j+1]){
                j++;
            }
            if(nums[j]>nums[i]){
                swap(nums,i,j);
            }else{
                break;
            }
            i=j;
            j=2*i+1;
        }
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogn)$，建堆的时间代价是 $O(n)$，删除的总代价是 $O(klogn)$，因为 `k<n`，故渐进时间复杂为 $O(n+klogn)=O(nlogn)$。
- 空间复杂度：$O(logn)$，即递归使用栈空间的空间代价。
### 解法三:
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int len=nums.length;
        return search(nums,0,nums.length-1,len-k);
    }
    public int search(int[] nums,int start,int end,int index){
        int ret=partition(nums,start,end);
        if(ret==index){
            return nums[index];
        }else if(ret>index){
            return search(nums,start,ret-1,index);
        }else{
            return search(nums,start+1,end,index);
        }
    }
    public int partition(int[] nums,int start,int end){
        int pivot=nums[start];
        while(start<end){
            while(start<end&&nums[end]>pivot){
                end--;
            }
            nums[start]=nums[end];
            while(start<end&&nums[start]<=pivot){
                start++;
            }
            nums[end]=nums[start];
        }
        nums[start]=pivot;
        return start;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，如上文所述，证明过程可以参考「《算法导论》9.2：期望为线性的选择算法」。
- 空间复杂度：$O(logn)$，递归使用栈空间的空间代价的期望为 $O(logn)$。
