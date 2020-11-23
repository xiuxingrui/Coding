# [LeetCode452.用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)
## 题目描述
在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。

一支弓箭可以沿着 `x` 轴从不同点完全垂直地射出。在坐标 `x` 处射出一支箭，若有一个气球的直径的开始和结束坐标为 `x_start`，`x_end`， 且满足  `x_start ≤ x ≤ x_end`，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

给你一个数组 `points` ，其中 `points [i] = [x_start,x_end]` ，返回引爆所有气球所必须射出的最小弓箭数。

- `0 <= points.length <= 10^4`
- `points[i].length == 2`
- `-2^31 <= x_start < x_end <= 2^31 - 1`

### 示例
```
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
```
```
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
```
```
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
```
```
输入：points = [[1,2]]
输出：1
```
```
输入：points = [[2,3],[2,3]]
输出：1
```
## 题解
此题既可以`start`排序，也可以`end`排序。`start`排序时不如`end`排序简单，而且有同学反映贪心思想的题目基本都是`end`排序。

我用的`start`排序，所以先说`start`排序：主要思路是如果两个区间完全不挨着，那肯定得多用一根箭；如果两个区间挨着就不用多加一根，但3个及以上区间挨着时就一定要注意挨着的这些区间的最小`end`，如果下一个区间的`start`大于这个最小`end`，那即使区间挨着也得多加一根箭。两个区间挨着的话肯定一根箭就够了，3个及以上挨着可就不一定了。
```
         Λ
  [-------|]
     [----|------] //这俩哥们一个箭够了
        [-|---------] //如果第三个是这样的，那一根箭还是够的
          |  [-------]  //如果是这样的呢，不行了，因为这个的start小于第一行那个end了，所以得再加一根
```
如果用`end`排序，那么一个最大的好处就是区间列表里第一项的`end`是最小的`end`，这样就不用再上上文提到的那样去找最小`end`了。在`end`排序下，只有一种情况需要多加箭，那就是`next_start > cur_end`时，其他情况统统都是一支箭搞定，这样从代码上还是思路上都简化了不少。
```
       Λ  
  [-----|]     
[-------|---]  
  [-----|---]
     [--|---]
        |  [-----]  // 只有此时需要再来一根
```

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) {
            return 0;
        }
        Arrays.sort(points, new Comparator<int[]>() {
            public int compare(int[] point1, int[] point2) {
                if (point1[1] > point2[1]) {
                    return 1;
                } else if (point1[1] < point2[1]) {
                    return -1;
                } else {
                    return 0;
                }
            }
        });
        int pos = points[0][1];
        int ans = 1;
        for (int[] balloon: points) {
            if (balloon[0] > pos) {
                pos = balloon[1];
                ++ans;
            }
        }
        return ans;
    }
}

```
按照`start`排序：
```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points==null||points.length==0){
            return 0;
        }
        Arrays.sort(points,(o1,o2)->(Integer.compare(o1[0],o2[0])));
        int end=points[0][1];
        int cnt=1,len=points.length;
        for(int i=1;i<len;i++){
            if(points[i][0]<=end){
                end=Math.min(points[i][1],end);
            }else{
                cnt++;
                end=points[i][1];
            }
        }
        return cnt;
    }
}
```
或
```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points==null||points.length==0){
            return 0;
        }
        Arrays.sort(points, new Comparator<int[]>() {
            public int compare(int[] point1, int[] point2) {
                if (point1[0] > point2[0]) {
                    return 1;
                } else if (point1[0] < point2[0]) {
                    return -1;
                } else {
                    return 0;
                }
            }
        });
        int end=points[0][1];
        int cnt=1,len=points.length;
        for(int i=1;i<len;i++){
            if(points[i][0]<=end){
                end=Math.min(points[i][1],end);
            }else{
                cnt++;
                end=points[i][1];
            }
        }
        return cnt;
    }
}
```
自己的解法，过不了`[[-2147483646,-2147483645],[2147483646,2147483647]]`这个案例：
```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points==null||points.length==0){
            return 0;
        }
        Arrays.sort(points,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                if(a[0]!=b[0]){
                    return a[0]-b[0];
                }else{
                    return b[1]-a[1];
                }
            }
        });
        int end=points[0][1];
        int cnt=1,len=points.length;
        for(int i=1;i<len;i++){
            if(points[i][0]<=end){
                end=Math.min(points[i][1],end);
            }else{
                cnt++;
                end=points[i][1];
            }
        }
        return cnt;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(nlogn)$，其中 `n` 是数组 `points` 的长度。排序的时间复杂度为 $O(nlogn)$，对所有气球进行遍历并计算答案的时间复杂度为 $O(n)$，其在渐进意义下小于前者，因此可以忽略。
- 空间复杂度：$O(logn)$，即为排序需要使用的栈空间。