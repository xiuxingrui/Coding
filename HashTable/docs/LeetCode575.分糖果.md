# [LeetCode575.分糖果](https://leetcode-cn.com/problems/distribute-candies/)
## 题目描述
给定一个偶数长度的数组，其中不同的数字代表着不同种类的糖果，每一个数字代表一个糖果。你需要把这些糖果平均分给一个弟弟和一个妹妹。返回妹妹可以获得的最大糖果的种类数。

- 数组的长度为`[2, 10,000]`，并且确定为偶数。
- 数组中数字的大小在范围`[-100,000, 100,000]`内。

### 示例
```
输入: candies = [1,1,2,2,3,3]
输出: 3
解析: 一共有三种种类的糖果，每一种都有两个。
     最优分配方案：妹妹获得[1,2,3],弟弟也获得[1,2,3]。这样使妹妹获得糖果的种类数最多。
```
```
输入: candies = [1,1,2,3]
输出: 2
解析: 妹妹获得糖果[2,3],弟弟获得糖果[1,1]，妹妹有两种不同的糖果，弟弟只有一种。这样使得妹妹可以获得的糖果种类数最多。
```
## 题解
### 解法一
找到唯一元素数量的另一种方法是遍历给定 `candies` 数组的所有元素，并继续将元素放入集合中。通过集合的属性，它将只包含唯一的元素。最后，我们可以计算集合中元素的数量，例如 `count`。要返回的值将再次由 `min(count,n/2)` 给出。其中 `n` 表示 `candies` 数组的大小。

```java
class Solution {
    public int distributeCandies(int[] candies) {
        int len=candies.length;
        HashSet<Integer> set=new HashSet<>();
        for(int num:candies){
            set.add(num);
        }
        if(len/2>=set.size()){
            return set.size();
        }else{
            return len/2;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$。整个 `candies` 数组只遍历一次。这里，`n` 表示 `candies` 数组的大小。
- 空间复杂度：$O(n)$,在最坏的情况下，`set` 的大小为 `n`。

### 解法二
我们可以对给定的`candies`数组进行排序，并通过比较排序数组的相邻元素来找出唯一的元素。对于找到的每个新元素（与前一个元素不同），我们需要更新 `count`。最后，我们可以将所需结果返回为 `min(n/2,count)`，如前面的方法所述。

```java
public class Solution {
    public int distributeCandies(int[] candies) {
        Arrays.sort(candies);
        int count = 1;
        for (int i = 1; i < candies.length && count < candies.length / 2; i++)
            if (candies[i] > candies[i - 1])
                count++;
        return count;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogn)$。排序需要 $O(nlogn)$ 的时间。
- 空间复杂度：$O(1)$。
