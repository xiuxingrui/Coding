# [剑指Offer57-II.和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)
## 题目描述
输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

`1 <= target <= 10^5`
### 示例
```
输入：target = 9
输出：[[2,3,4],[4,5]]
```
```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```
## 题解
### 解法一
```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        List<int[]> list=new ArrayList<>();
        int maxRight=target/2+1;//右区间的最大值
        int maxLen=(int)Math.sqrt(target*2);//长度最大值
        for(int i=1;i<=maxRight;i++){
            for(int j=2;j<=maxLen;j++){
                int right=i+j-1;
                int sum=(i+right)*j/2;
                if(sum>target){
                    break;
                }
                if(sum==target){
                    int[] temp=new int[j];
                    int idx=0;
                    for(int k=i;k<=right;k++){
                        temp[idx++]=k;
                    }
                    list.add(temp);
                }
            }
        }
        int[][] ans=new int[list.size()][];
        int i=0;
        for(int[] temp:list){
            ans[i++]=temp;
        }
        return ans;
    }
}
```
### 解法二
设滑动窗口的左边界为 `i`，右边界为 `j`，则滑动窗口框起来的是一个左闭右开区间 `[i,j)`。注意，为了编程的方便，滑动窗口一般表示成一个左闭右开区间。在一开始，`i=1`,`j=1`滑动窗口位于序列的最左侧，窗口大小为零。

滑动窗口的重要性质是：窗口的左边界和右边界永远只能向右移动，而不能向左移动。这是为了保证滑动窗口的时间复杂度是 $O(n)$。

在这道题中，我们关注的是滑动窗口中所有数的和。当滑动窗口的右边界向右移动时，也就是 `j = j + 1`，窗口中多了一个数字 `j`，窗口的和也就要加上 `j`。当滑动窗口的左边界向右移动时，也就是 `i = i + 1`，窗口中少了一个数字 `i`，窗口的和也就要减去 `i`。滑动窗口只有 右边界向右移动（扩大窗口） 和 左边界向右移动（缩小窗口） 两个操作。

- 当窗口的和小于 `target` 的时候，窗口的和需要增加，所以要扩大窗口，窗口的右边界向右移动
- 当窗口的和大于 `target` 的时候，窗口的和需要减少，所以要缩小窗口，窗口的左边界向右移动
- 当窗口的和恰好等于 `target` 的时候，我们需要记录此时的结果。设此时的窗口为 `[i,j)`，那么我们已经找到了一个 `i` 开头的序列，也是唯一一个 `i` 开头的序列，接下来需要找 `i+1` 开头的序列，所以窗口的左边界要向右移动

大于`target`，左边界需要右移，但是右边界不需要移动。
```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        int i = 1; // 滑动窗口的左边界
        int j = 1; // 滑动窗口的右边界
        int sum = 0; // 滑动窗口中数字的和
        List<int[]> res = new ArrayList<>();

        while (i <= target / 2) {
            if (sum < target) {
                // 右边界向右移动
                sum += j;
                j++;
            } else if (sum > target) {
                // 左边界向右移动
                sum -= i;
                i++;
            } else {
                // 记录结果
                int[] arr = new int[j-i];
                for (int k = i; k < j; k++) {
                    arr[k-i] = k;
                }
                res.add(arr);
                // 左边界向右移动
                sum -= i;
                i++;
            }
        }

        return res.toArray(new int[res.size()][]);
    }
}
```