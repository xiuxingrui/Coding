# [LeetCode1095.山脉数组中查找目标值](https://leetcode-cn.com/problems/find-in-mountain-array/)
## 题目描述
（这是一个 交互式问题 ）

给你一个 山脉数组 `mountainArr`，请你返回能够使得 `mountainArr.get(index)` 等于 `target` 最小 的下标 `index` 值。

如果不存在这样的下标 `index`，就请返回 -1。

何为山脉数组？如果数组 `A` 是一个山脉数组的话，那它满足如下条件：

首先，`A.length >= 3`

其次，在 `0 < i < A.length - 1` 条件下，存在 `i` 使得：
- `A[0] < A[1] < ... A[i-1] < A[i]`
- `A[i] > A[i+1] > ... > A[A.length - 1]`

你将 不能直接访问该山脉数组，必须通过 `MountainArray` 接口来获取数据：

- `MountainArray.get(k)` - 会返回数组中索引为`k` 的元素（下标从 0 开始）
- `MountainArray.length()` - 会返回该数组的长度

注意：

对 `MountainArray.get` 发起超过 100 次调用的提交将被视为错误答案。此外，任何试图规避判题系统的解决方案都将会导致比赛资格被取消。

- `3 <= mountain_arr.length() <= 10000`
- `0 <= target <= 10^9`
- `0 <= mountain_arr.get(index) <= 10^9`

### 示例
```
输入：array = [1,2,3,4,5,3,1], target = 3
输出：2
解释：3 在数组中出现了两次，下标分别为 2 和 5，我们返回最小的下标 2。
```
```
输入：array = [0,1,2,4,2,1], target = 3
输出：-1
解释：3 在数组中没有出现，返回 -1。
```
## 题解
自然地，求解这道题可以分为 3 步：

第 1 步：先找到山顶元素 `mountaintop` 所在的索引。

第 2 步：在前有序且升序数组中找 `target` 所在的索引，如果找到了，就返回，如果没有找到，就执行第 3 步；

第 3 步：如果步骤 2 找不到，就在后有序且降序数组中找 `target` 所在的索引。


```java
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */
 
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int len=mountainArr.length();
        int mountainTop=getMountainTop(mountainArr,0,len-1);
        int index=binarySearch(mountainArr,target,0,mountainTop,true);
        if(index!=-1){
            return index;
        }
        return binarySearch(mountainArr,target,mountainTop+1,len-1,false);
    }
    public int getMountainTop(MountainArray mountainArr,int start,int end){
        int i=start,j=end;
        while(i<j){
            int mid=i+(j-i)/2;
            if(mountainArr.get(mid)>mountainArr.get(mid+1)){
                j=mid;
            }else{
                i=mid+1;
            }
        }
        return i;
    }
    public int binarySearch(MountainArray mountainArr,int target,int start,int end,boolean flag){
        int i=start,j=end;
        while(i<=j){
            int mid=i+(j-i)/2;
            if(mountainArr.get(mid)==target){
                return mid;
            }
            if(mountainArr.get(mid)>target){
                if(flag){
                    j=mid-1;
                }else{
                    i=mid+1;
                }
            }else{
                if(flag){
                    i=mid+1;
                }else{
                    j=mid-1;
                }
            }
        }
        return -1;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(logn)$，我们进行了三次二分搜索，每次的时间复杂度都为 $O(logn)$。
- 空间复杂度：$O(1)$，只需要常数的空间存放若干变量。
