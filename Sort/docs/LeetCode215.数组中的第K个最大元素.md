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
### 解法一:
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
### 解法二:
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
### 解法三
优先队列:

大顶堆时间复杂度$O(NlogN)$，小顶堆是 $O(NlogK)$

优先队列的思路是很朴素的。因为第 `K` 大元素，其实就是整个数组排序以后后半部分最小的那个元素。因此，我们可以维护一个有 `K` 个元素的最小堆：

1. 如果当前堆不满，直接添加；
2. 堆满的时候，如果新读到的数小于等于堆顶，肯定不是我们要找的元素，只有新都到的数大于堆顶的时候，才将堆顶拿出，然后放入新读到的数，进而让堆自己去调整内部结构。

说明：这里最合适的操作其实是 `replace`，即直接把新读进来的元素放在堆顶，然后执行下沉（`siftDown`）操作。`Java` 当中的 `PriorityQueue` 没有提供这个操作，只好先 `poll()` 再 `offer()`。

假设数组有 `len` 个元素。

把 `len` 个元素都放入一个最小堆中，然后再 `pop()` 出 `len - k` 个元素，此时最小堆只剩下 `k` 个元素，堆顶元素就是数组中的第 `k` 个最大元素。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> queue=new PriorityQueue<>();
        for(int i=0;i<k;i++){
            queue.offer(nums[i]);
        }
        for(int i=k;i<nums.length;i++){
            int top=queue.peek();
            if(nums[i]>top){
                queue.poll();
                queue.offer(nums[i]);
            }
        }
        return queue.peek();
    }
}
```
自己的实现：
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        heapify(nums,k);
        for(int i=k;i<nums.length;i++){
            if(nums[i]>nums[0]){
                nums[0]=nums[i];
                siftDown(nums,0,k-1);
            }
        }
        return nums[0];
    }
    public void heapify(int[] nums,int k){
        for(int i=(k-1)/2;i>=0;i--){
            siftDown(nums,i,k-1);
        }
    }
    public void siftDown(int[] nums,int start,int end){
        int i=start,j=i*2+1;
        while(j<=end){
            if(j+1<=end&&nums[j+1]<nums[j]){
                j=j+1;
            }
            if(nums[j]<nums[i]){
                int temp=nums[i];
                nums[i]=nums[j];
                nums[j]=temp;
            }
            i=j;
            j=i*2+1;
        }
    }
}
```
大顶堆写法:
```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        // 使用一个含有 len 个元素的最大堆，lambda 表达式应写成：(a, b) -> b - a
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(len, (a, b) -> b - a);
        for (int i = 0; i < len; i++) {
            maxHeap.add(nums[i]);
        }
        for (int i = 0; i < k - 1; i++) {
            maxHeap.poll();
        }
        return maxHeap.peek();
    }
}
```
