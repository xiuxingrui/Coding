# [剑指Offer40.最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
## 题目描述
输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

- `0 <= k <= arr.length <= 10000`
- `0 <= arr[i] <= 10000`
### 示例
```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```
```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

## 题解
### 解法一
```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(arr.length==0||k==0){
            return new int[0];
        }
        search(arr,0,arr.length-1,k);
        return Arrays.copyOf(arr, k);
    }
    public void search(int[] nums,int start,int end,int k){
        int pos=partition(nums,start,end);
        if(pos==k-1){
            return;
        }else if(pos<k-1){
            search(nums,pos+1,end,k);
        }else{
            search(nums,start,pos-1,k);
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
- 空间复杂度 $O(1)$，不需要额外空间。
- 时间复杂度的分析方法和快速排序类似。由于快速选择只需要递归一边的数组，时间复杂度小于快速排序，期望时间复杂度为 $O(n)$，最坏情况下的时间复杂度为$(n^2)$。

### 解法一
```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        int len=arr.length;
        int[] ans=new int[k];
        heapify(arr);
        for(int i=0;i<k;i++){
            ans[i]=arr[0];
            swap(arr,0,len-1-i);
            siftDown(arr,0,len-2-i);
        }
        return ans;
    }
    public void heapify(int[] nums){
        int len=nums.length;
        for(int i=(len-1)/2;i>=0;i--){
            siftDown(nums,i,len-1);
        }
    }
    public void siftDown(int[] nums,int start,int end){
        int i=start,j=2*i+1;
        while(j<=end){
            if(j+1<=end&&nums[j+1]<nums[j]){
                j++;
            }
            if(nums[i]>nums[j]){
                swap(nums,i,j);
            }else{
                break;
            }
            i=j;
            j=i*2+1;
        }
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
### 解法二:
本题是求前 `K`小，因此用一个容量为 `K` 的大根堆，每次 `poll` 出最大的数，那堆中保留的就是前 `K` 小啦（注意不是小根堆！小根堆的话需要把全部的元素都入堆，那是 $O(NlogN)$，就不是 $O(NlogK)$。
```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(arr.length==0||k==0){
            return new int[0];
        }
        PriorityQueue<Integer> queue=new PriorityQueue<>((a,b)->b-a);
        int[] ans=new int[k];
        for(int i=0;i<k;i++){
            queue.offer(arr[i]);
        }
        for(int i=k;i<arr.length;i++){
            int top=queue.peek();
            if(arr[i]<top){
                queue.poll();
                queue.offer(arr[i]);
            }
        }
        int cnt=0;
        for(int num:queue){
            ans[cnt++]=num;
        }
        return ans;
    }
}
```
自己的实现：
```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        heapify(arr,k);
        for(int i=k;i<arr.length;i++){
            if(arr[i]<arr[0]){
                arr[0]=arr[i];
                siftDown(arr,0,k-1);
            }
        }
        return Arrays.copyOfRange(arr,0,k);
    }
    public void heapify(int[] arr,int k){
        for(int i=(k-1)/2;i>=0;i--){
            siftDown(arr,i,k-1);
        }
    }
    public void siftDown(int[] arr,int start,int end){
        int i=start,j=2*i+1;
        while(j<=end){
            if(j+1<=end&&arr[j+1]>arr[j]){
                j=j+1;
            }
            if(arr[j]>arr[i]){
                int temp=arr[i];
                arr[i]=arr[j];
                arr[j]=temp;
            }
            i=j;
            j=2*i+1;
        }
    }
}
```
#### 复杂度分析
- 由于使用了一个大小为 `k` 的堆，空间复杂度为 $O(k)$；
- 入堆和出堆操作的时间复杂度均为 $O(logk)$，每个元素都需要进行一次入堆操作，故算法的时间复杂度为 $O(nlogk)$。


在面试中，另一个常常问的问题就是堆和快速选择有何优劣。看起来分治法的快速选择算法的时间、空间复杂度都优于使用堆的方法，但是要注意到快速选择算法的几点局限性：

第一，算法需要修改原数组，如果原数组不能修改的话，还需要拷贝一份数组，空间复杂度就上去了。

第二，算法需要保存所有的数据。如果把数据看成输入流的话，使用堆的方法是来一个处理一个，不需要保存数据，只需要保存 `k` 个元素的最大堆。而快速选择的方法需要先保存下来所有的数据，再运行算法。当数据量非常大的时候，甚至内存都放不下的时候，就麻烦了。所以当数据量大的时候还是用基于堆的方法比较好。

