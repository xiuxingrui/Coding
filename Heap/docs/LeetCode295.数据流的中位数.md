# [LeetCode295.数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)
## 题目描述
中位数是有序列表中间的数。如果列表长度是偶数，中位数则是中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- `void addNum(int num)` - 从数据流中添加一个整数到数据结构中。
- `double findMedian()` - 返回目前所有元素的中位数。

进阶:

- 如果数据流中所有整数都在 0 到 100 范围内，你将如何优化你的算法？
- 如果数据流中 99% 的整数都在 0 到 100 范围内，你将如何优化你的算法？

### 示例
```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

## 题解
给定一长度为 `N` 的无序数组，其中位数的计算方法：首先对数组执行排序（使用 $O(NlogN)$时间），然后返回中间元素即可（使用 $O(1)$时间）。

针对本题，根据以上思路，可以将数据流保存在一个列表中，并在添加元素时 保持数组有序 。此方法的时间复杂度为 $O(N)$，其中包括： 查找元素插入位置 $O(logN)$（二分查找）、向数组某位置插入元素 $O(N)$（插入位置之后的元素都需要向后移动一位）。

借助 堆 可进一步优化时间复杂度。

建立一个 小顶堆 `A` 和 大顶堆 `B` ，各保存列表的一半元素，且规定：

- `A` 保存 较大 的一半，长度为 `N/2`（ `N` 为偶数）或 `(N+1)/2`（ `N` 为奇数）；
- `B` 保存 较小 的一半，长度为 `N/2`（ `N` 为偶数）或 `(N−1)/2`（ `N` 为奇数）；

随后，中位数可仅根据 `A`,`B` 的堆顶元素计算得到。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201012154658.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201012155548.png)
算法流程：

设元素总数为 `N=m+n`，其中 `m` 和 `n` 分别为 `A` 和 `B` 中的元素个数。

`addNum(num)` 函数：
- 当 `m=n`（即 `N` 为 偶数）：需向 `A` 添加一个元素。实现方法：将新元素 `num` 插入至 `B` ，再将 `B` 堆顶元素插入至 `A` ；
- 当 `m≠n`（即 `N` 为 奇数）：需向 `B` 添加一个元素。实现方法：将新元素 `num` 插入至 `A` ，再将 `A` 堆顶元素插入至 `B` ；

假设插入数字 `num` 遇到情况 1. 。由于 `num`可能属于 “较小的一半” （即属于 `B` ），因此不能将 `nums`直接插入至 `A` 。而应先将 `num` 插入至 `B` ，再将 `B` 堆顶元素插入至 `A` 。这样就可以始终保持 `A` 保存较大一半、 `B` 保存较小一半。

`findMedian()` 函数：
- 当 `m=n`（ `N` 为 偶数）：则中位数为 ( `A` 的堆顶元素 + `B` 的堆顶元素 )/2。
- 当 `m≠n`（ `N` 为 奇数）：则中位数为 `A` 的堆顶元素。

```java
class MedianFinder {
    Queue<Integer> minHeap, maxHeap;
    public MedianFinder() {
        minHeap = new PriorityQueue<>(); // 小顶堆，保存较大的一半
        maxHeap = new PriorityQueue<>((x, y) -> (y - x)); // 大顶堆，保存较小的一半
    }
    public void addNum(int num) {
        //假定堆内元素个数总和为奇数，插大顶堆
        if(minHeap.size() != maxHeap.size()) {
            if(!minHeap.isEmpty()&&num>minHeap.peek()){
                int oldMin=minHeap.poll();
                minHeap.offer(num);
                num=oldMin;
            }
            maxHeap.offer(num);
        } else {
            if(!maxHeap.isEmpty()&&num<maxHeap.peek()){
                int oldMax=maxHeap.poll();
                maxHeap.offer(num);
                num=oldMax;
            }
            minHeap.offer(num);
        }
    }
    public double findMedian() {
        return minHeap.size() != maxHeap.size() ? minHeap.peek() : (minHeap.peek() + maxHeap.peek()) / 2.0;
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```
### 复杂度分析
- 时间复杂度：
  - 查找中位数 $O(1)$： 获取堆顶元素使用 $O(1)$时间；
  - 添加数字 $O(logN)$： 堆的插入和弹出操作使用 $O(logN)$时间。
- 空间复杂度 $O(N)$ ： 其中 `N` 为数据流中的元素数量，小顶堆 `A` 和大顶堆 `B` 最多同时保存 `N` 个元素。