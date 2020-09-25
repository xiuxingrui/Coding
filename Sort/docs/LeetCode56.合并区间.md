# [LeetCode56.合并区间](https://leetcode-cn.com/problems/merge-intervals/)
## 题目描述
给出一个区间的集合，请合并所有重叠的区间。

- `intervals[i][0] <= intervals[i][1]`
### 示例
```
输入: intervals = [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```
```
输入: intervals = [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```
## 题解
### 解法一
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914134320.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914134332.png)

可以被合并的区间一定是有交集的区间，前提是区间按照左端点排好序，这里的交集可以是一个点（例如例 2）。

只需要对所有的区间按照左端点升序排序，然后遍历。

- 如果`当前遍历到的区间的左端点 > 结果集中最后一个区间的右端点`，说明它们没有交集，此时把区间添加到结果集；
- 如果`当前遍历到的区间的左端点 <= 结果集中最后一个区间的右端点`，说明它们有交集，此时产生合并操作，即：对结果集中最后一个区间的右端点更新（取两个区间的最大值）。
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        // 先按照区间起始位置排序
        Arrays.sort(intervals, (v1, v2) -> v1[0] - v2[0]);
        // 遍历区间
        int[][] res = new int[intervals.length][2];
        int idx = -1;
        for (int[] interval: intervals) {
            // 如果结果数组是空的，或者当前区间的起始位置 > 结果数组中最后区间的终止位置，
            // 则不合并，直接将当前区间加入结果数组。
            if (idx == -1 || interval[0] > res[idx][1]) {
                res[++idx] = interval;
            } else {
                // 反之将当前区间合并至结果数组的最后区间
                res[idx][1] = Math.max(res[idx][1], interval[1]);
            }
        }
        return Arrays.copyOf(res, idx + 1);
    }
}
```
或
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals,(v1,v2)->(v1[0]-v2[0]));
        List<int[]> ans=new ArrayList<>();
        for(int i=0;i<intervals.length;i++){
            if(ans.isEmpty()||intervals[i][0]>ans.get(ans.size()-1)[1]){
                ans.add(intervals[i]);
            }else{
                ans.get(ans.size()-1)[1]=Math.max(ans.get(ans.size()-1)[1],intervals[i][1]);
            }
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```
### 解法二
自己的写法:

区间分别为 `[x1, y1` 和 `[x2,y2]`。根据数学知识我们可以知道，当 `min(y1, y2) > max(x1, x2)` 时，这两条线段有交集。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals==null||intervals.length==0||intervals[0].length==0){
            return new int[0][2];
        }
        List<int[]> ans=new ArrayList<>();
        Arrays.sort(intervals,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                if(a[0]!=b[0]){
                    return a[0]-b[0];
                }else{
                    return a[1]-b[1];
                }
            }
        });
        for(int i=0;i<intervals.length-1;i++){
            if(Math.min(intervals[i][1],intervals[i+1][1])>=Math.max(intervals[i][0],intervals[i+1][0])){
                intervals[i+1][0]=Math.min(intervals[i][0],intervals[i+1][0]);
                intervals[i+1][1]=Math.max(intervals[i][1],intervals[i+1][1]);
            }else{
                ans.add(intervals[i]);
            }
        }
        ans.add(intervals[intervals.length-1]);
        int size=ans.size();
        int[][] nums=new int[size][2];
        for(int i=0;i<size;i++){
            nums[i]=ans.get(i);
        }
        return nums;
        //return ans.toArray(new int[ans.size()][]);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(nlogn)$，其中 `n` 为区间的数量。除去排序的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的 $O(nlogn)$。
- 空间复杂度：$O(logn)$，其中 `n` 为区间的数量。这里计算的是存储答案之外，使用的额外空间。$O(logn)$ 即为排序所需要的空间复杂度。


