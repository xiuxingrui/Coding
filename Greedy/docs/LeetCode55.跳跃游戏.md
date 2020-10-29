# [LeetCode55.跳跃游戏](https://leetcode-cn.com/problems/jump-game/)
## 题目描述
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

### 示例
```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```
```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```
## 题解
### 解法一
我们可以用贪心的方法解决这个问题。

设想一下，对于数组中的任意一个位置 `y`，我们如何判断它是否可以到达？根据题目的描述，只要存在一个位置 `x`，它本身可以到达，并且它跳跃的最大长度为 `x+nums[x]`，这个值大于等于 `y`，即 `x+nums[x]≥y`，那么位置 `y` 也可以到达。

换句话说，对于每一个可以到达的位置 `x`，它使得 `x+1`,`x+2`,⋯,`x+nums[x]`这些连续的位置都可以到达。

这样以来，我们依次遍历数组中的每一个位置，并实时维护 最远可以到达的位置。对于当前遍历到的位置 `x`，如果它在 最远可以到达的位置 的范围内，那么我们就可以从起点通过若干次跳跃到达该位置，因此我们可以用 `x+nums[x]` 更新 最远可以到达的位置。

在遍历的过程中，如果 最远可以到达的位置 大于等于数组中的最后一个位置，那就说明最后一个位置可达，我们就可以直接返回 `True` 作为答案。反之，如果在遍历过程中位置`i`小于最远可达的位置，我们就返回 `False` 作为答案。
```java
public class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int rightmost = 0;
        for (int i = 0; i < n; ++i) {
            if (i <= rightmost) {
                rightmost = Math.max(rightmost, i + nums[i]);
                if (rightmost >= n - 1) {
                    return true;
                }
            }else{
                return false;
            }
        }
        return true;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 为数组的大小。只需要访问 `nums` 数组一遍，共 `n` 个位置。
- 空间复杂度：$O(1)$，不需要额外的空间开销。

### 解法二
`DP`:
```java
class Solution {
    public boolean canJump(int[] nums) {
        int len=nums.length;
        boolean[] dp=new boolean[len];
        dp[0]=true;
        for(int i=1;i<len;i++){
            for(int j=0;j<i;j++){
                if(dp[j]==true&&j+nums[j]>=i){
                    dp[i]=true;
                    break;
                }
            }
        }
        return dp[len-1];
    }
}
```