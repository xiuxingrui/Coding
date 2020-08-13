# [LeetCode剑指Offer66.构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)
## 题目描述
给定一个数组 `A[0,1,…,n-1]`，请构建一个数组 `B[0,1,…,n-1]`，其中 `B` 中的元素 `B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]`。不能使用除法。

提示：

- 所有元素乘积之和不会溢出 32 位整数
- `a.length <= 100000`
### 示例
```
输入: [1,2,3,4,5]
输出: [120,60,40,30,24]
```
## 题解
使用两个数组 `L` 和 `R`，`L[i]` 表示 `a[i]` 左边的乘积，而 `R[i]` 表示 `a[i]` 右边的乘积，那么 `L` 和 `R` 对应位置的乘积就是所求结果。

具体来说，我们可以使用三趟遍历完成：

1. 正向遍历，`L[i]=L[i−1]×a[i−1]`；
2. 反向遍历，`R[j]=R[j+1]×a[j+1]`；
3. 正向遍历，`result[i]=L[i]=L[i]×R[i]`；

注：当 `i=0` 时，`result[0]=L[0]=1×R[0]`。即 `L[0]=1`，同理 `R[n−1]=1`。

```java
class Solution {
    public int[] constructArr(int[] a) {
        if(a==null||a.length==0){
            return new int[0];
        }
        int len=a.length;
        int[] left=new int[len];
        int[] right=new int[len];
        for(int i=0;i<len;i++){
            if(i==0){
                left[i]=1;
            }else{
                left[i]=left[i-1]*a[i-1];
            }
        }
        for(int i=len-1;i>=0;i--){
            if(i==len-1){
                right[i]=1;
            }else{
                right[i]=right[i+1]*a[i+1];
            }
        }
        for(int i=0;i<len;i++){
            left[i]=left[i]*right[i];
        }
        return left;
    }
}
```
只需在第二步反向遍历 过程中直接计算出结果即可。
```java
class Solution {
    public int[] constructArr(int[] a) {
        if(a==null||a.length==0){
            return new int[0];
        }
        int len=a.length;
        int[] ans=new int[len];
        for(int i=0;i<len;i++){
            if(i==0){
                ans[i]=1;
            }else{
                ans[i]=ans[i-1]*a[i-1];
            }
        }
        int r=1;
        for(int i=len-1;i>=0;i--){
            ans[i]=ans[i]*r;
            r*=a[i];
        }
        return ans;
    }
}
```
### 复杂度分析
改进前:
- 时间复杂度：$O(3N)～O(N)$，遍历了三次数组。
- 空间复杂度：$O(2N)～O(N)$，使用了两个数组。
改进后
- 时间复杂度：$O(2N)～O(N)$，遍历了两次数组。
- 空间复杂度：$O(N)$，使用了一个数组。


