# [LeetCode354.俄罗斯套娃信封问题](https://leetcode-cn.com/problems/russian-doll-envelopes/)
## 题目描述
给定一些标记了宽度和高度的信封，宽度和高度以整数对形式 `(w, h)` 出现。当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算最多能有多少个信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

### 示例
```java
输入: envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出: 3 
解释: 最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。
```
## 题解
### 解法一
这道题目其实是最长递增子序列（Longes Increasing Subsequence，简写为 LIS）的一个变种，因为很显然，每次合法的嵌套是大的套小的，相当于找一个最长递增的子序列，其长度就是最多能嵌套的信封个数。

假设我们知道了信封套娃顺序，那么从里向外的顺序必须是按 `w` 升序排序的子序列。

在对信封按 `w` 进行排序以后，我们可以找到 `h` 上最长递增子序列的长度。

我们考虑输入 `[[1，3]，[1，4]，[1，5]，[2，3]]`，如果我们直接对 `h` 进行 `LIS` 算法，我们将会得到 `[3，4，5]`，显然这不是我们想要的答案，因为 `w` 相同的信封是不能够套娃的。

为了解决这个问题。我们可以按 `w` 进行升序排序，若 `w` 相同则按 `h` 降序排序。则上述输入排序后为 `[[1，5]，[1，4]，[1，3]，[2，3]]`，再对 `h` 进行 `LIS` 算法可以得到 `[5]`，长度为 1，是正确的答案。这个例子可能不明显。

我们将输入改为 `[[1，5]，[1，4]，[1，2]，[2，3]]`。则提取 `h` 为 `[5，4，2，3]`。我们对 `h` 进行 LIS 算法将得到 `[2，3]`，是正确的答案。

算法为:

先对宽度 `w` 进行升序排序，如果遇到 `w` 相同的情况，则按照高度 `h` 降序排序。之后把所有的 `h` 作为一个数组，在这个数组上计算 `LIS` 的长度就是答案。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200901152122.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914010636.png)

这个解法的关键在于，对于宽度 `w` 相同的数对，要对其高度 `h` 进行降序排序。因为两个宽度相同的信封不能相互包含的，逆序排序保证在 `w` 相同的数对中最多只选取一个。

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if(envelopes==null||envelopes.length==0){
            return 0;
        }
        Arrays.sort(envelopes,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                return a[0]==b[0]?b[1]-a[1]:a[0]-b[0];
            }
        });
        int dp[]=new int[envelopes.length];
        int maxLen=1;
        for(int i=0;i<envelopes.length;i++){
            dp[i]=1;
            for(int j=0;j<i;j++){
                if(envelopes[i][1]>envelopes[j][1]){
                    dp[i]=Math.max(dp[i],dp[j]+1);
                }
            }
            maxLen=Math.max(maxLen,dp[i]);
        }
        return maxLen;
    }
}
```
### 解法二:二分法优化
```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if(envelopes==null||envelopes.length==0){
            return 0;
        }
        Arrays.sort(envelopes,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                return a[0]==b[0]?b[1]-a[1]:a[0]-b[0];
            }
        });
        int[] temp=new int[envelopes.length];
        for(int i=0;i<envelopes.length;i++){
            temp[i]=envelopes[i][1];
        }
        int[] dp=new int[envelopes.length];
        int maxLen=1;
        dp[0]=temp[0];
        for(int i=1;i<envelopes.length;i++){
            int idx=binarySearch(dp,maxLen,temp[i]);
            if(idx==maxLen){
                dp[idx]=temp[i];
                maxLen++;
            }else{
                dp[idx]=temp[i];
            }
        }
        return maxLen;
    }
    public int binarySearch(int[] nums,int end,int target){
        int left=0,right=end;
        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]>=target){
                right=mid;
            }else{
                left=mid+1;
            }
        }
        return left;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(NlogN)$。其中 `N` 是输入数组的长度。排序和 `LIS` 算法都是 $O(NlogN)$。
- 空间复杂度：$O(N)$





