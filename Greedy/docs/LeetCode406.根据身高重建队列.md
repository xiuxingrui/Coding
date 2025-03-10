# [LeetCode406.根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)
## 题目描述
假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对`(h, k)`表示，其中h是这个人的身高，`k`是排在这个人前面且身高大于或等于`h`的人数。 编写一个算法来重建这个队列。

注意：
总人数少于1100人。

### 示例
```
输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```
## 题解
让我们从最简单的情况下思考，当队列中所有人的`(h,k)` 都是相同的高度 `h`，只有 `k` 不同时，解决方案很简单：每个人在队列的索引 `index = k`。

即使不是所有人都是同一高度，这个策略也是可行的。因为个子矮的人相对于个子高的人是 “看不见” 的，所以可以先安排个子高的人。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201029204717.png)

上图中我们先安排身高为 7 的人，将它放置在与 `k` 值相等的索引上；再安排身高为 6 的人，同样的将它放置在与 `k` 值相等的索引上。

该策略可以递归进行：

- 将最高的人按照 `k` 值升序排序，然后将它们放置到输出队列中与 `k` 值相等的索引位置上。
- 按降序取下一个高度，同样按 `k` 值对该身高的人升序排序，然后逐个插入到输出队列中与 `k` 值相等的索引位置上。


直到完成为止。

算法：

- 排序：按高度降序排列。
- 在同一高度的人中，按 `k` 值的升序排列。
- 逐个地把它们放在输出队列中，索引等于它们的 `k` 值。
- 返回输出队列

矮的人相对于高的人是看不见的：因为`k`的值是前面比自己高或等高的人的个数。先处理高的人，满足`k`的条件后，再处理矮的人是不会破坏这个条件的。

相对矮的人插入对应的`k`位置的话，可以保证结果中的`k`条件不被破坏（因为此时结果中高的人已经站好了，矮的无法影响他们），但是因为矮的人要占据一个位置的，所以后面的人只能往后面挪动。

排序后，每个人的`p[1]`是不可能大于排序后的下标的

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201029204851.png)
```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people,new Comparator<int[]>(){
            @Override
            public int compare(int[] a,int[] b){
                if(a[0]!=b[0]){
                    return b[0]-a[0];
                }else{
                    return a[1]-b[1];
                }
            }
        });
        List<int[]> list=new LinkedList<>();
        for(int[] p:people){
            list.add(p[1],p);
        }
        return list.toArray(new int[people.length][2]);
    }
}
```
另一种解释：

按照`hi`为第一关键字降序，`ki`为第二关键字升序进行排序。如果我们按照排完序后的顺序，依次将每个人放入队列中，那么当我们放入第 `i` 个人时：
- 第 `0,⋯,i−1` 个人已经在队列中被安排了位置，他们只要站在第 `i` 个人的前面，就会对第 `i` 个人产生影响，因为他们都比第 `i` 个人高；
- 而第 `i+1,⋯,n−1` 个人还没有被放入队列中，并且他们无论站在哪里，对第 `i` 个人都没有任何影响，因为他们都比第 `i` 个人矮。

在这种情况下，我们无从得知应该给后面的人安排多少个「空」位置，因此就不能沿用方法一。但我们可以发现，后面的人既然不会对第 `i` 个人造成影响，我们可以采用「插空」的方法，依次给每一个人在当前的队列中选择一个插入的位置。也就是说，当我们放入第 `i` 个人时，只需要将其插入队列中，使得他的前面恰好有 `ki`个人即可。

### 复杂度分析
- 时间复杂度：$(N^2)$。排序使用了 $O(NlogN)$ 的时间，每个人插入到输出队列中需要 $O(k)$ 的时间，其中 `k` 是当前输出队列的元素个数。总共的时间复杂度为$O(N^2)$。
- 空间复杂度：$O(N)$，输出队列使用的空间。


