# [剑指Offer61.扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)
## 题目描述
从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，`A`为1，`J`为11，`Q`为12，`K`为13，而大、小王为 0 ，可以看成任意数字。`A` 不能视为 14。

- 数组长度为 5 
- 数组的数取值为 `[0, 13]` .
### 示例
```
输入: [1,2,3,4,5]
输出: True
```
```
输入: [0,0,1,2,5]
输出: True
```
## 题解
### 解法一
排序：

排序之后扑克牌就有序了，我们就可以直接判断相邻两张牌之间需要多少个大王或小王来填补。

- 如果需要填补大小王的数量，大于已有大小王的数量，则返回 `false` .
- 相反，如果需要填补大小王的数量，小于或等于已有大小王的数量，则返回 `true` .

例如：
```
扑克牌： 0 1 4 5 6
我们发现 1 和 4 之间需要 4 - 1 - 1 张大王或小王来填补
但大王或小王只有 1 张，所以它不可以构成顺子
```

```java
class Solution {
    public boolean isStraight(int[] nums) {
        Arrays.sort(nums);
        int cntZero=0,idx=0;
        while(nums[idx]==0){
            cntZero++;
            idx++;
        }
        int cnt=0;
        while(idx<nums.length-1){
            if(nums[idx]==nums[idx+1]){
                return false;
            }
            cnt+=nums[idx+1]-nums[idx]-1;
            idx++;
        }
        return cntZero>=cnt;
    }
}
```
### 解法二
有一串连续的数字（无重复），这串数字中最大值为 `m`， 最小值为 `n` ，问你这串数字中一共有多少个数字？:`m−n+1`

同样，如果我们能够知道 5 张扑克牌中的最大值 `maxValue` 和最小值 `minValue`，那我们就知道，要使它为顺子需要 `maxValue−minValue+1`张牌。

- 在查找 `maxValue` 和 `minValue` 过程中，跳过大小王 0。
- 如果 `maxValue−minValue+1>5`，说明题目给的 5 张牌不足以构成顺子，返回 `false`.
  - 即使里面有大小王，也不够用来填补使它构成顺子。
- 如果 `maxValue−minValue+1<=5`，说明 5 张牌足以构成顺子，返回 `true`。
  - 里面的大小王填补在合适位置即可。
同时，我们再定义一个标志数组判断是否有重复数字，发现重复数字直接返回 `false` 即可。

根据题意，此 5 张牌是顺子的 充分条件 如下：

- 除大小王外，所有牌 无重复 ；
- 设此 5 张牌中最大的牌为 `max` ，最小的牌为 `min` （大小王除外），则需满足：
  - `max−min<5`
因而，可将问题转化为：此 5 张牌是否满足以上两个条件？

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201008152914.png)

方法一：集合 `Set` + 遍历

- 遍历五张牌，遇到大小王（即 0 ）直接跳过。
- 判别重复： 利用 `Set` 实现遍历判重， `Set` 的查找方法的时间复杂度为 $O(1)$ ；
- 获取最大 / 最小的牌： 借助辅助变量 `max` 和 `min` ，遍历统计即可。

```java
class Solution {
    public boolean isStraight(int[] nums) {
        Set<Integer> repeat = new HashSet<>();
        int max = 0, min = 14;
        for(int num : nums) {
            if(num == 0) continue; // 跳过大小王
            max = Math.max(max, num); // 最大牌
            min = Math.min(min, num); // 最小牌
            if(repeat.contains(num)) return false; // 若有重复，提前返回 false
            repeat.add(num); // 添加此牌至 Set
        }
        return max - min < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
    }
}
```
- 时间复杂度 $O(N)=O(5)=O(1)$： 其中 `N` 为 `nums` 长度，本题中 `N≡5`；遍历数组使用 $O(N)$ 时间。
- 空间复杂度 $O(N)=O(5)=O(1)$： 用于判重的辅助 Set 使用 $O(N)$ 额外空间。

方法二：排序 + 遍历

- 先对数组执行排序。
- 判别重复： 排序数组中的相同元素位置相邻，因此可通过遍历数组，判断 `nums[i]=nums[i+1]`是否成立来判重。
- 获取最大 / 最小的牌： 排序后，数组末位元素 `nums[4]` 为最大牌；元素 `nums[joker]` 为最小牌，其中 `joker` 为大小王的数量。

```java
class Solution {
    public boolean isStraight(int[] nums) {
        int joker = 0;
        Arrays.sort(nums); // 数组排序
        for(int i = 0; i < 4; i++) {
            if(nums[i] == 0) joker++; // 统计大小王数量
            else if(nums[i] == nums[i + 1]) return false; // 若有重复，提前返回 false
        }
        return nums[4] - nums[joker] < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
    }
}
```
- 时间复杂度 $O(NlogN)=O(5log5)=O(1)$： 其中 `N` 为 `nums` 长度，本题中 `N≡5`；数组排序使用 $O(NlogN)$时间。
- 空间复杂度 $O(1)$： 变量 `joker` 使用 $O(1)$大小的额外空间。
