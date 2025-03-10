# [LeetCode875.爱吃香蕉的珂珂](https://leetcode-cn.com/problems/koko-eating-bananas/)
## 题目描述
珂珂喜欢吃香蕉。这里有 `N` 堆香蕉，第 `i` 堆中有 `piles[i]` 根香蕉。警卫已经离开了，将在 `H` 小时后回来。

珂珂可以决定她吃香蕉的速度 `K` （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 `K` 根。如果这堆香蕉少于 `K` 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。  

珂珂喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在 `H` 小时内吃掉所有香蕉的最小速度 `K`（`K` 为整数）。

### 示例
```
输入: piles = [3,6,7,11], H = 8
输出: 4

输入: piles = [30,11,23,4,20], H = 5
输出: 30

输入: piles = [30,11,23,4,20], H = 6
输出: 23
```
### 提示
- `1 <= piles.length <= 10^4`
- `piles.length <= H <= 10^9`
- `1 <= piles[i] <= 10^9`
## 题解
```java
class Solution {
    public int minEatingSpeed(int[] piles, int H) {
        int maxValue=1;
        for(int p:piles){
            maxValue=p>maxValue?p:maxValue;
        }
        int left=1,right=maxValue;
        while(left<right){
            int mid=left+(right-left)/2;
            if(helper(piles,mid,H)){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        return left;
    }
    public boolean helper(int[] piles,int K,int H){
        int count=0;
        for(int p:piles){
            if(p%K==0){
                count+=p/K;
            }else{
                count+=p/K+1;
            }
        }
        return count<=H;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N \log W)$，其中 `N` 是香蕉堆的数量，`W` 最大的香蕉堆的大小。
- 空间复杂度：$O(1)$。
