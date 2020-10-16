# [LeetCode977.有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)
## 题目描述
给定一个按非递减顺序排序的整数数组 `A`，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。

- `1 <= A.length <= 10000`
- `10000 <= A[i] <= 10000`
- `A` 已按非递减顺序排序。
### 示例
```
输入：[-4,-1,0,3,10]
输出：[0,1,9,16,100]
```
```
输入：[-7,-3,2,3,11]
输出：[4,9,9,49,121]
```
## 题解
### 解法一
```java
class Solution {
    public int[] sortedSquares(int[] A) {
        int[] ans=new int[A.length];
        int idx=0,i=0,j=0;
        if(A[0]>=0){
            for(int k=0;k<A.length;k++){
                ans[idx++]=A[k]*A[k];
            }
            return ans;
        }
        while(i<A.length&&A[i]<0){
            i++;
        }
        if(i==A.length){
            for(int k=A.length-1;k>=0;k--){
                ans[idx++]=A[k]*A[k];
            }
            return ans;
        }
        j=i-1;
        while(j>=0&&i<A.length){
            if(-A[j]<A[i]){
                ans[idx++]=A[j]*A[j];
                j--;
            }else{
                ans[idx++]=A[i]*A[i];
                i++;
            }
        }
        while(j>=0){
            ans[idx++]=A[j]*A[j];
            j--;
        };
        while(i<A.length){
            ans[idx++]=A[i]*A[i];
            i++;
        }
        return ans;
    }
}
```
### 解法二
```java
class Solution {
    public int[] sortedSquares(int[] A) {
        for(int i=0;i<A.length;i++){
            A[i]=A[i]*A[i];
        }
        Arrays.sort(A);
        return A;
    }
}
```
### 解法三
```java
class Solution {
    public int[] sortedSquares(int[] A) {
        int[] ans=new int[A.length];
        ans[0]=A[0]*A[0];
        for(int i=1;i<A.length;i++){
            int j=i-1;
            while(j>=0&&ans[j]>A[i]*A[i]){
                ans[j+1]=ans[j];
                j--;
            }
            ans[j+1]=A[i]*A[i];
        }
        return ans;
    }
}
```