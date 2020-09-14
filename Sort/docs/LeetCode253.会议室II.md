# [LeetCode253.会议室II](https://leetcode-cn.com/problems/meeting-rooms-ii/)
## 题目描述
给定一个会议时间安排的数组，每个会议时间都会包括开始和结束的时间 `[[s1,e1],[s2,e2],...]` `(si < ei)`，为避免会议冲突，同时要考虑充分利用会议室资源，请你计算至少需要多少间会议室，才能满足这些会议安排。

### 示例
```
输入: [[0, 30],[5, 10],[15, 20]]
输出: 2
```
```
输入: [[7,10],[2,4]]
输出: 1
```
## 题解
从一组想要开会但还没有分配会议室的人员的角度来看这个问题。他们会怎么做？

这组人会依次查看会议室，检查是否有任何会议室是空闲的。如果他们找到一间空闲的会议室，就会在那个房间里开始开会。否则,他们会等待一个会议室空出来。一旦会议室空出来,他们就会占用它。

我们无法按任意顺序处理给定的会议。处理会议的最基本方式是按其 开始时间 顺序排序，这也是我们采取的顺序。这就是我们将遵循的顺序。毕竟，在担心下午5：00的会议之前，你肯定应该先安排上午9：00的会议，不是吗？

让我们对一个示例问题进行一次模拟，来分析我们的算法应该能有效地执行哪些操作。

考虑下面的会议时间 `(1, 10), (2, 7), (3, 19), (8, 12), (10, 20), (11, 30)` 。前一部分表示会议开始时间，后一部分表示结束时间。按照会议开始时间顺序考虑。图一展示了前三个会议，每个会议都由于冲突而需要新房间。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914143940.png)

后面的三个会议开始占用现有的房间。然而，最后的会议需要一个新房间。总而言之，我们需要四个房间来容纳所有会议。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914144016.png)

排序过程很容易，但对每个会议，我们如何高效地找出是否有房间可用？任意时刻，我们都有多个可能占用的房间，只要我们能在有新会议需要时就找到一个空闲房间，我们并不需要关心到底有哪些房间是空闲的。

一个朴素的方法是，每当有新会议时，就遍历所有房间，查看是否有空闲房间。

但是,通过使用优先队列（或最小堆）堆数据结构,我们可以做得更好。

我们可以将所有房间保存在最小堆中,堆中的键值是会议的结束时间，而不用手动迭代已分配的每个房间并检查房间是否可用。

这样，每当我们想要检查有没有 任何 房间是空的，只需要检查最小堆堆顶的元素，它是最先开完会腾出房间的。

如果堆顶的元素的房间并不空闲，那么其他所有房间都不空闲。这样，我们就可以直接开一个新房间。

1. 按照 开始时间 对会议进行排序。
2. 初始化一个新的 最小堆，将第一个会议的结束时间加入到堆中。我们只需要记录会议的结束时间，告诉我们什么时候房间会空。
3. 对每个会议，检查堆的最小元素（即堆顶部的房间）是否空闲。
   1. 若房间空闲，则从堆顶拿出该元素，将我们处理的会议的结束时间加回到堆中。
   2. 若房间不空闲。开新房间，并加入到堆中。
4. 处理完所有会议后，堆的大小即为开的房间数量。这就是容纳这些会议需要的最小房间数。
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        PriorityQueue<Integer> pqueue=new PriorityQueue<>();
        // PriorityQueue<Integer> pqueue =new PriorityQueue<Integer>(intervals.length,new Comparator<Integer>() {
        //     public int compare(Integer a, Integer b) {
        //         return a - b;
        //     }
        // });
        Arrays.sort(intervals,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                if(a[0]!=b[0]){
                    return a[0]-b[0];
                }else{
                    return a[1]-b[1];
                }
            }
        });
        int cnt=0;
        for(int i=0;i<intervals.length;i++){
            if(pqueue.isEmpty()==true||intervals[i][0]<pqueue.peek()){
                cnt++;
                pqueue.offer(intervals[i][1]);
            }else{
                pqueue.poll();
                pqueue.offer(intervals[i][1]);
            }
        }
        return cnt;
    }
}
```
或者直接`return`优先队列的容量即可。
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        PriorityQueue<Integer> pqueue=new PriorityQueue<>();
        // PriorityQueue<Integer> pqueue =new PriorityQueue<Integer>(intervals.length,new Comparator<Integer>() {
        //     public int compare(Integer a, Integer b) {
        //         return a - b;
        //     }
        // });
        Arrays.sort(intervals,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                if(a[0]!=b[0]){
                    return a[0]-b[0];
                }else{
                    return a[1]-b[1];
                }
            }
        });
        for(int i=0;i<intervals.length;i++){
            if(pqueue.isEmpty()==true||intervals[i][0]<pqueue.peek()){
                pqueue.offer(intervals[i][1]);
            }else{
                pqueue.poll();
                pqueue.offer(intervals[i][1]);
            }
        }
        return pqueue.size();
    }
}
```
### 复杂度分析
- 时间复杂度：$O(NlogN)$。
  - 时间开销主要有两部分。第一部分是数组的 排序 过程，消耗 $O(NlogN)$ 的时间。数组中有 `N` 个元素。 
  - 接下来是 最小堆 占用的时间。在最坏的情况下，全部 `N` 个会议都会互相冲突。在任何情况下，我们都要向堆执行 `N` 次插入操作。在最坏的情况下，我们要对堆进行 `N` 次查找并删除最小值操作。总的时间复杂度为 $(NlogN)$，因为查找并删除最小值操作只消耗 $O(logN)$ 的时间。
- 空间复杂度：$O(N)$。额外空间用于建立 最小堆 。在最坏的情况下，堆需要容纳全部 `N` 个元素。因此空间复杂度为 $O(N)$。