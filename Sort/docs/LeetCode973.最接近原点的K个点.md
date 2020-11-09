# [LeetCode973.最接近原点的K个点](https://leetcode-cn.com/problems/k-closest-points-to-origin/)
## 题目描述
我们有一个由平面上的点组成的列表 `points`。需要从中找出 `K` 个距离原点 `(0, 0)` 最近的点。

（这里，平面上两点之间的距离是欧几里德距离。）

你可以按任何顺序返回答案。除了点坐标的顺序之外，答案确保是唯一的。

- `1 <= K <= points.length <= 10000`
- `-10000 < points[i][0] < 10000`
- `-10000 < points[i][1] < 10000`

### 示例
```
输入：points = [[1,3],[-2,2]], K = 1
输出：[[-2,2]]
解释： 
(1, 3) 和原点之间的距离为 sqrt(10)，
(-2, 2) 和原点之间的距离为 sqrt(8)，
由于 sqrt(8) < sqrt(10)，(-2, 2) 离原点更近。
我们只需要距离原点最近的 K = 1 个点，所以答案就是 [[-2,2]]。
```
```
输入：points = [[3,3],[5,-1],[-2,4]], K = 2
输出：[[3,3],[-2,4]]
（答案 [[-2,4],[3,3]] 也会被接受。）
```
## 题解
### 解法一
我们也可以借鉴快速排序的思想。

快速排序中的划分操作每次执行完后，都能将数组分成两个部分，其中小于等于分界值 `pivot` 的元素都会被放到左侧部分，而大于 `pivot` 的元素都都会被放到右侧部分。与快速排序不同的是，在本题中我们可以根据 `K` 与 `pivot` 下标的位置关系，只处理划分结果的某一部分（而不是像快速排序一样需要处理两个部分）。

在整个过程结束之后，第 `K` 个距离最小的点恰好就在数组 `points` 中的第 `K` 个位置，并且其左侧的所有点的距离都小于它。此时，我们就找到了前 `K` 个距离最小的点。

```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        search(points,K,0,points.length-1);
        return Arrays.copyOfRange(points,0,K);
    }
    public void search(int[][] points,int K,int start,int end){
        int pos=partition(points,start,end);
        if(pos==K-1){
            return;
        }
        if(pos<K-1){
            search(points,K,pos+1,end);
        }else{
            search(points,K,start,pos-1);
        }
    }
    public int partition(int[][] points,int start,int end){
        int[] pivot=points[start];
        int i=start,j=end;
        while(i<j){
            while(i<j&&(Math.pow(points[j][0],2)+Math.pow(points[j][1],2)>Math.pow(pivot[0],2)+Math.pow(pivot[1],2))){
                j--;
            }
            points[i]=points[j];
            while(i<j&&(Math.pow(points[i][0],2)+Math.pow(points[i][1],2)<=Math.pow(pivot[0],2)+Math.pow(pivot[1],2))){
                i++;
            }
            points[j]=points[i];
        }
        points[i]=pivot;
        return i;
    }
}
```
#### 复杂度分析
- 时间复杂度：期望为 $O(n)$，其中 `n` 是数组 `points` 的长度。由于证明过程很繁琐，所以不再这里展开讲。具体证明可以参考《算法导论》第 9 章第 2 小节。

最坏情况下，时间复杂度为 $O(n^2)$。具体地，每次的划分点都是最大值或最小值，一共需要划分 `n−1` 次，而一次划分需要线性的时间复杂度，所以最坏情况下时间复杂度为 $O(n^2)$。
- 空间复杂度：期望为 $O(logn)$，即为递归调用的期望深度。

最坏情况下，空间复杂度为 $O(n)$，此时需要划分 `n−1` 次，对应递归的深度为 `n−1` 层，所以最坏情况下时间复杂度为 $O(n)$。

然而注意到代码中的递归都是「尾递归」，因此如果编译器支持尾递归优化，那么空间复杂度总为 $O(1)$。即使不支持尾递归优化，我们也可以很方便地将上面的代码改成循环迭代的写法。

### 解法二
堆：

我们可以使用一个优先队列（大根堆）实时维护前 `K` 个最小的距离平方。

首先我们将前 `K` 个点的编号（为了方便最后直接得到答案）以及对应的距离平方放入优先队列中，随后从第 `K+1` 个点开始遍历：如果当前点的距离平方比堆顶的点的距离平方要小，就把堆顶的点弹出，再插入当前的点。当遍历完成后，所有在优先队列中的点就是前 `K` 个距离最小的点。

写法一：
```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((v1,v2)->  (v2[0]*v2[0]+ v2[1]*v2[1]) -   (v1[0]*v1[0]+ v1[1]*v1[1]) );

        for(int i=0;i<points.length;i++){
            if(i<K){
                pq.offer(points[i]);
            }
            else if((pq.peek()[0] * pq.peek()[0]+ pq.peek()[1] * pq.peek()[1]) > (points[i][0]*points[i][0]+ points[i][1]*points[i][1]))
            {
                pq.poll();
                pq.offer(points[i]);
            }
        }

        int[][] ans = new int[K][2];
        
        for(int i=0;i<K;i++){
            ans[i] = pq.poll();
        }

        return ans;

    }
}
```
写法二：
```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] point1, int[] point2) {
                return (point2[0] * point2[0] + point2[1] * point2[1]) - (point1[0] * point1[0] + point1[1] * point1[1]);
            }
        });

        for(int i=0;i<points.length;i++){
            if(i<K){
                pq.offer(points[i]);
            }
            else if((pq.peek()[0] * pq.peek()[0]+ pq.peek()[1] * pq.peek()[1]) > (points[i][0]*points[i][0]+ points[i][1]*points[i][1]))
            {
                pq.poll();
                pq.offer(points[i]);
            }
        }

        int[][] ans = new int[K][2];
        
        for(int i=0;i<K;i++){
            ans[i] = pq.poll();
        }

        return ans;

    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogK)$，其中 `n` 是数组 `points` 的长度。由于优先队列维护的是前 `K` 个距离最小的点，因此弹出和插入操作的单次时间复杂度均为 $O(logK)$。在最坏情况下，数组里 `n` 个点都会插入，因此时间复杂度为 $O(nlogK)$。
- 空间复杂度：$O(K)$，因为优先队列中最多有 `K` 个点。
### 解法三
排序：

当我们计算出每个点到原点的欧几里得距离的平方后，本题和「剑指 Offer 40. 最小的k个数」是完全一样的题。

为什么是欧几里得距离的「平方」？这是因为欧几里得距离并不一定是个整数，在进行计算和比较时可能会引进误差；但它的平方一定是个整数，这样我们就无需考虑误差了。

将每个点到原点的欧几里得距离的平方从小到大排序后，取出前 `K` 个即可。

写法一：
```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        Arrays.sort(points, new Comparator<int[]>() {
            public int compare(int[] point1, int[] point2) {
                return (point1[0] * point1[0] + point1[1] * point1[1]) - (point2[0] * point2[0] + point2[1] * point2[1]);
            }
        });
        return Arrays.copyOfRange(points, 0, K);
    }
}
```
写法二：
```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
       Arrays.sort(points,(v1,v2)->   (v1[0]*v1[0]+ v1[1]*v1[1])- (v2[0]*v2[0]+ v2[1]*v2[1])  );
       return Arrays.copyOfRange(points,0,K);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogn)$，其中 `n` 是数组 `points` 的长度。算法的时间复杂度即排序的时间复杂度。
- 空间复杂度：$O(logn)$，排序所需额外的空间复杂度为 $O(logn)$。
