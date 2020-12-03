# [LeetCode912.排序数组](https://leetcode-cn.com/problems/sort-an-array/)
## 题目描述
给你一个整数数组 `nums`，请你将该数组升序排列。

- `1 <= nums.length <= 50000`
- `-50000 <= nums[i] <= 50000`

### 示例
```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```
```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```
## 题解
### 归并排序
```java
class Solution {
    public int[] sortArray(int[] nums) {
        if (nums == null || nums.length == 0) return null;
        mergeSort(nums, 0, nums.length - 1);
        return nums;
    }
    //分
    public void mergeSort(int[] nums, int start, int end){
        if(start >= end ) return ;

        int mid=start+(end-start)/2;
        mergeSort(nums, start,mid);
        mergeSort(nums,mid+1,end);
        // 如果数组的这个子区间本身有序，无需合并
        if (nums[mid] <= nums[mid + 1]) {
            return;
        }
        merge(nums, start,mid,end);
    }
    //合
    public void merge(int[] nums, int start,int mid,int end){
        int[] temp = new int[end-start+1];
        int left= start;
        int right= mid + 1;
        int index = 0;
        while(left<= mid && right <= end){
            if(nums[left] <=nums[right]){
                temp[index++] = nums[left++];
            }else{
                temp[index++] = nums[right++];
            }
        }

        while(left<= mid){
           temp[index++] = nums[left++];
       }
        while(right<= end){
           temp[index++] = nums[right++];
       }
        for(int i=0;i<index;i++){
            nums[start+i] = temp[i];
        }
    }
}
```
### 快速排序
#### 版本一
```java
class Solution {
    public int[] sortArray(int[] nums) {
        if (nums == null || nums.length == 0) return null;
        quickSort(nums, 0, nums.length - 1);
        return nums;
    }
    public void quickSort(int[] nums,int start,int end){
        if(start>=end){
            return;
        }
        int pos=partition(nums,start,end);
        quickSort(nums,start,pos-1);
        quickSort(nums,pos+1,end);
    }
    public int partition(int[] nums,int start,int end){
        int pivot=nums[start];
        while (start<end) {
            while (start<end&&nums[end]>pivot) end--;
            nums[start]=nums[end];
            while (start<end&&nums[start]<=pivot) start++;
            nums[end] = nums[start];
        }
        nums[start] = pivot;
        return start;
    }
}
```
#### 版本二：

整个划分函数 `partition` 主要涉及两个指针 `i` 和 `j`，一开始 `i = l - 1`，`j = l`。我们需要实时维护两个指针使得任意时候，对于任意数组下标 `k`，我们有如下条件成立：
1. `l≤k≤i` 时，`nums[k]≤pivot`。
2. `i+1≤k≤j−1` 时，`nums[k]>pivot`。
3. `k==r` 时，`nums[k]=pivot`。

我们每次移动指针 `j` ，如果 `nums[j]>pivot`，我们只需要继续移动指针 `j` ，即能使上述三个条件成立，否则我们需要将指针 `i` 加一，然后交换 `nums[i]`和 `nums[j]`，再移动指针 `j` 才能使得三个条件成立。

当 `j` 移动到 `r−1` 时结束循环，此时我们可以由上述三个条件知道 `[l,i]` 的数都小于等于主元 `pivot`，`[i+1,r−1]` 的数都大于主元 `pivot`，那么我们只要交换 `nums[i+1]`和 `nums[r]`，即能使得 `[l,i+1]` 区间的数都小于 `[i+2,r]` 区间的数，完成一次划分，且分界值下标为 `i+1`，返回即可。

```java
class Solution {
    public int[] sortArray(int[] nums) {
        if (nums == null || nums.length == 0) return null;
        quickSort(nums, 0, nums.length - 1);
        return nums;
    }
    public void quickSort(int[] nums,int start,int end){
        if(start>=end){
            return;
        }
        int pos=partition(nums,start,end);
        quickSort(nums,start,pos-1);
        quickSort(nums,pos+1,end);
    }
    public int partition(int[] nums,int start,int end){
        int pivot = nums[end];
        int i = start - 1;
        for (int j = start; j <= end - 1; ++j) {
            if (nums[j] <= pivot) {
                i = i + 1;
                swap(nums,i,j);
            }
        }
        swap(nums,i+1,end);
        return i + 1;
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
#### 版本三:
```java
class Solution {
    public int[] sortArray(int[] nums) {
        if (nums == null || nums.length == 0) return null;
        quickSort(nums, 0, nums.length - 1);
        return nums;
    }
    public void quickSort(int[] nums,int start,int end){
        if(start>=end){
            return;
        }
        int pos=partition(nums,start,end);
        quickSort(nums,start,pos-1);
        quickSort(nums,pos+1,end);
    }
    public int partition(int[] nums,int start,int end){
        int pivot = nums[start];
        int lt = start;
        // 循环不变量：
        // all in [start + 1, lt] < pivot
        // all in [lt + 1, i) >= pivot
        for (int i = start + 1; i <= end; i++) {
            if (nums[i] < pivot) {
                lt++;
                swap(nums, i, lt);
            }
        }
        swap(nums,start,lt);
        return lt;
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
### 堆排序
```java
public class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        // 将数组整理成堆
        heapify(nums);

        // 循环不变量：区间 [0, i] 堆有序
        for (int i = len - 1; i >= 1;i--) {
            // 把堆顶元素（当前最大）交换到数组末尾
            swap(nums, 0, i);
            // 逐步减少堆有序的部分
            // 下标 0 位置下沉操作，使得区间 [0, i] 堆有序
            siftDown(nums, 0, i-1);
        }
        return nums;
    }

    /**
     * 将数组整理成堆（堆有序）
     *
     * @param nums
     */
    public void heapify(int[] nums) {
        int len = nums.length;
        // 只需要从 i = (len - 1) / 2 这个位置开始逐层下移
        for (int i = (len - 1) / 2; i >= 0; i--) {
            siftDown(nums, i, len - 1);
        }
    }

    /**
     * @param nums
     * @param low   当前下沉元素的下标
     * @param high  [0, high] 是 nums 的有效部分
     */
    public void siftDown(int[] nums, int low, int high) {
        int i=low,j=2*i+1;
        while (j<=high) {
            if (j + 1 <= high && nums[j + 1] > nums[j]) {
                j++;
            }
            if (nums[j] > nums[i]) {
                swap(nums, j, i);
            } else {
                break;
            }
            i = j;
            j=2*i+1;
        }
    }

    private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
    }
}
```
插入元素:
```java
public void siftUp(int[] nums,int low, int high) {
	int i = high, j = i / 2;//i为欲调整结点，j为其父亲
	while (j >= low) {//父亲在[low,high]范围内
		//父亲权值小于欲调整结点i的权值
		if (nums[j] < nums[i]) {
			swap(nums,i,j);//交换父亲和欲调整结点
			i = j;
			j = i / 2;
		}
		else {
			break;//父亲权值比欲调整结点i的权值大，调整结束
		}
	}
}
//添加元素x
public void insert(int[] nums,int x) {
	nums[++n] = x;//让元素个数加1，然后将数组末位赋值为x
	upAdjust(0, n);//向上调整新加入的结点n
}
```
### 冒泡排序
```java
class Solution {
    public int[] sortArray(int[] nums) {
        bubbleSort(nums);
        return nums;
    }
    public void bubbleSort(int[] nums){
        int len=nums.length;
        boolean sorted=false;
        for(int i=0;i<len-1;i++){
            for(int j=0;j<len-1-i;j++){
                if(nums[j]>nums[j+1]){
                    sorted=true;
                    swap(nums,j,j+1);
                }
            }
            if(sorted==false){
                return;
            }
        }
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
### 插入排序
#### 版本一
```java
class Solution {
    public int[] sortArray(int[] nums) {
        insertSort(nums);
        return nums;
    }
    public void insertSort(int[] nums){
        int len=nums.length;
        for(int i=1;i<len;i++){
            int temp=nums[i];
            int j=i;
            while(j>0&&nums[j-1]>temp){
                nums[j]=nums[j-1];
                j--;
            }
            nums[j]=temp;
        }
    }
}
```
#### 版本二
```java
class Solution {
    public int[] sortArray(int[] nums) {
        insertSort(nums);
        return nums;
    }
    public void insertSort(int[] nums){
        int len=nums.length;
        for(int i=1;i<len;i++){
            int temp=nums[i];
            int j=i-1;
            while(j>=0&&nums[j]>temp){
                nums[j+1]=nums[j];
                j--;
            }
            nums[j+1]=temp;
        }
    }
}
```
### 选择排序
```java
class Solution {
    public int[] sortArray(int[] nums) {
        selectionSort(nums);
        return nums;
    }
    public void selectionSort(int[] nums){
        int len=nums.length;
        for(int i=0;i<len-1;i++){
            int minIndex=i;
            for(int j=i+1;j<len;j++){
                if(nums[j]<nums[minIndex]){
                    minIndex=j;
                }
            }
            swap(nums,i,minIndex);
        }
    }
    public void swap(int[] nums,int index1,int index2){
        int temp=nums[index1];
        nums[index1]=nums[index2];
        nums[index2]=temp;
    }
}
```
