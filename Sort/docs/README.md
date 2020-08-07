# [剑指Offer40.最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
## 

## 题解
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