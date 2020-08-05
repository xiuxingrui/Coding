# [剑指Offer51.数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)
## 题目描述
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

限制：
`0 <= 数组长度 <= 50000`
```
输入: [7,5,6,4]
输出: 5
```
## 题解
```java
class Solution {
    public int reversePairs(int[] nums) {
        if(nums.length<2){
            return 0;
        }
        int len=nums.length;
        return countPairs(nums,0,len-1);
    }
    public int countPairs(int[] nums,int start,int end){
        if(start>=end){
            return 0;
        }
        int mid=start+(end-start)/2;
        int leftPair=countPairs(nums,start,mid);
        int rightPair=countPairs(nums,mid+1,end);
        if(nums[mid] <= nums[mid + 1]){
            return leftPair+rightPair;
        }
        return leftPair+rightPair+merge(nums,start,mid,end);
    }
    public int merge(int[] nums,int start,int mid,int end){
        int left=start,right=mid+1,leftLength=mid-start+1,rightLength=end-mid;
        int[] temp=new int[end-start+1];
        int index=0,cnt=0;
        while(left<=mid&&right<=end){
            if(nums[left]<=nums[right]){
                temp[index++]=nums[left++];
                leftLength--;
            }else{
                temp[index++]=nums[right++];
                cnt+=leftLength;
            }
        }
        while(left<=mid){
            temp[index++]=nums[left++];
        }
        while(right<=end){
            temp[index++]=nums[right++];
        }
        for(int i=0;i<index;i++){
            nums[start+i]=temp[i];
        }
        return cnt;
    }
}
```
另一种写法:
```java
class Solution {
    public int reversePairs(int[] nums) {
        if(nums.length<2){
            return 0;
        }
        int len=nums.length;
        return countPairs(nums,0,len-1);
    }
    public int countPairs(int[] nums,int start,int end){
        if(start>=end){
            return 0;
        }
        int mid=start+(end-start)/2;
        int leftPair=countPairs(nums,start,mid);
        int rightPair=countPairs(nums,mid+1,end);
        if(nums[mid] <= nums[mid + 1]){
            return leftPair+rightPair;
        }
        return leftPair+rightPair+merge(nums,start,mid,end);
    }
    public int merge(int[] nums,int start,int mid,int end){
        int left=start,right=mid+1;
        int[] temp=new int[end-start+1];
        int index=0,cnt=0;
        while(left<=mid&&right<=end){
            if(nums[left]<=nums[right]){
                temp[index++]=nums[left++];
            }else{
                temp[index++]=nums[right++];
                cnt+=mid-left+1;
            }
        }
        while(left<=mid){
            temp[index++]=nums[left++];
        }
        while(right<=end){
            temp[index++]=nums[right++];
        }
        for(int i=0;i<index;i++){
            nums[start+i]=temp[i];
        }
        return cnt;
    }
}
```
在左边数组归并时进行统计:
```java
class Solution {
    public int reversePairs(int[] nums) {
        if(nums.length<2){
            return 0;
        }
        int len=nums.length;
        return countPairs(nums,0,len-1);
    }
    public int countPairs(int[] nums,int start,int end){
        if(start>=end){
            return 0;
        }
        int mid=start+(end-start)/2;
        int leftPair=countPairs(nums,start,mid);
        int rightPair=countPairs(nums,mid+1,end);
        if(nums[mid] <= nums[mid + 1]){
            return leftPair+rightPair;
        }
        return leftPair+rightPair+merge(nums,start,mid,end);
    }
    public int merge(int[] nums,int start,int mid,int end){
        int left=start,right=mid+1;
        int[] temp=new int[end-start+1];
        int index=0,cnt=0;
        while(left<=mid&&right<=end){
            if(nums[left]<=nums[right]){
                temp[index++]=nums[left++];
                cnt+=right-mid-1;
            }else{
                temp[index++]=nums[right++];
            }
        }
        while(left<=mid){
            temp[index++]=nums[left++];
            cnt+=end-mid;
        }
        while(right<=end){
            temp[index++]=nums[right++];
        }
        for(int i=0;i<index;i++){
            nums[start+i]=temp[i];
        }
        return cnt;
    }
}
```
### 复杂度
记序列长度为 `n`。
- 时间复杂度：同归并排序 $O(nlogn)$。
- 空间复杂度：同归并排序 $O(n)$，因为归并排序需要用到一个临时数组。