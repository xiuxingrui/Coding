# [LeetCode1013.将数组分成和相等的三个部分](https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum/)
## 题目描述
给你一个整数数组 `A`，只有可以将其划分为三个和相等的非空部分时才返回 `true`，否则返回 `false`。

形式上，如果可以找出索引 `i+1 < j` 且满足 `A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1]` 就可以将数组三等分。

- `3 <= A.length <= 50000`
- `-10^4 <= A[i] <= 10^4`

### 示例
```
输入：[0,2,1,-6,6,-7,9,1,2,0,1]
输出：true
解释：0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1
```
```
输入：[0,2,1,-6,6,7,9,-1,2,0,1]
输出：false
```
```
输入：[3,3,6,5,-2,2,5,1,-9,4]
输出：true
解释：3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
```
## 题解
- 首先计算数组 `A` 中所有数字总和 `sum`
- 遍历数组 `A` 查找和为 `sum/3` 的子数组个数
- 如果找到了三个这样的子数组则返回 `true`
- 找不到三个就返回 `false`

```java
class Solution {
    public boolean canThreePartsEqualSum(int[] A) {
        int sum=0;
        for(int num:A){
            sum+=num;
        }
        if(sum%3!=0){
            return false;
        }
        int count=0,subSum=0;
        for(int i=0;i<A.length;i++){
            subSum+=A[i];
            if(subSum==sum/3){
                count++;
                subSum=0;
            }
            if(count==3){
                return true;
            }
        }
        return false;
    }
}
```
`count == 2` 不就可以返回 `true` 了吗？ 因为已经找到了两个和为 `sum / 3` 的子数组，那剩下的不就是第三个了嘛！

解答：

其实不是这样的，因为找到了两个子数组后可能恰好到达了数组末尾，此时就不符合要求了，例如: `[1, -1, 1, -1]` .

自己的解法：
```java
class Solution {
    public boolean canThreePartsEqualSum(int[] A) {
        int sum=0;
        for(int num:A){
            sum+=num;
        }
        if(sum%3!=0){
            return false;
        }
        int tempSum=0;
        for(int i=0;i<A.length;i++){
            tempSum+=A[i];
            if(tempSum==sum/3){
                int temp=0;
                for(int j=i+1;j<A.length-1;j++){
                    temp+=A[j];
                    if(temp==sum/3){
                        return true;
                    }
                }
            }
        }
        return false;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是数组 `A` 的长度。我们最多只需要遍历一遍数组就可以得到答案。
- 空间复杂度：$O(1)$。我们只需要使用额外的索引变量 `i`，`j` 以及一些存储数组信息的变量。
