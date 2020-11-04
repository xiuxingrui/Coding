# [LeetCode57.插入区间](https://leetcode-cn.com/problems/insert-interval/)
## 题目描述
给出一个无重叠的 ，按照区间起始端点排序的区间列表。

在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

### 示例
```java
输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]
```
```
输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```
## 题解
### 解法一
```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        if(intervals==null||intervals.length==0){
            int[][] res=new int[1][2];
            res[0]=newInterval;
            return res;
        }
        int[][] temp=new int[intervals.length+1][2];
        int idx=0;
        boolean flag=true;
        for(int i=0;i<intervals.length;i++){
            if(flag&&newInterval[0]<intervals[i][0]){
                temp[idx++]=newInterval;
                flag=false;
            }
            temp[idx++]=intervals[i];
        }
        if(flag){
            temp[idx]=newInterval;
        }
        List<int[]> ans=new ArrayList<>();
        for(int i=0;i<temp.length;i++){
            if(ans.isEmpty()||temp[i][0]>ans.get(ans.size()-1)[1]){
                ans.add(temp[i]);
            }else{
                ans.get(ans.size()-1)[1]=Math.max(ans.get(ans.size()-1)[1],temp[i][1]);
            }
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```
或
```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        if(intervals==null||intervals.length==0){
            int[][] res=new int[1][2];
            res[0]=newInterval;
            return res;
        }
        List<int[]> list=new ArrayList<>();
        boolean flag=true;
        for(int i=0;i<intervals.length;i++){
            if(flag&&newInterval[0]<intervals[i][0]){
                list.add(newInterval);
                flag=false;
            }
            list.add(intervals[i]);
        }
        if(flag){
            list.add(newInterval);
        }
        List<int[]> ans=new ArrayList<>();
        for(int i=0;i<list.size();i++){
            if(ans.isEmpty()||list.get(i)[0]>ans.get(ans.size()-1)[1]){
                ans.add(list.get(i));
            }else{
                ans.get(ans.size()-1)[1]=Math.max(ans.get(ans.size()-1)[1],list.get(i)[1]);
            }
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```
### 解法二
对于区间 `S1=[l1,r1]`和 `S2=[l2,r2]`，如果它们之间没有重叠（没有交集），说明要么 `S1`在 `S2`的左侧，此时有 `r1<l2`；要么 `S1`在 `S2`的右侧，此时有 `l1>r2`。

如果 `r1<l2`和 `l1>r2`二者均不满足，说明 `S1`和 `S2`必定有交集，它们的交集即为

`[max(l1,l2),min(r1,r2)]`

并集即为

`[min(l1,l2),max(r1,r2)]`

在给定的区间集合`X`互不重叠的前提下，当我们需要插入一个新的区间 `S=[left,right]`时，我们只需要：

- 找出所有与区间 `S` 重叠的区间集合 `X'`；
- 将`X'`中的所有区间连带上区间 `S` 合并成一个大区间；
- 最终的答案即为不与`X'`重叠的区间以及合并后的大区间。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201104142817.png)

这样做的正确性在于，给定的区间集合中任意两个区间都是没有交集的，因此所有需要合并的区间，就是所有与区间 `S` 重叠的区间。

并且，在给定的区间集合已经按照左端点排序的前提下，所有与区间 `S` 重叠的区间在数组 `intervals` 中下标范围是连续的，因此我们可以对所有的区间进行一次遍历，就可以找到这个连续的下标范围。

当我们遍历到区间`[li,ri]`时：

- 如果 `ri<left`，说明 `[li,ri]`与`S`不重叠并且在其左侧，我们可以直接将 `[li,ri]`加入答案；
- 如果 `li>right`，说明 `[li,ri]`与 `S` 不重叠并且在其右侧，我们可以直接将 `[li,ri]`加入答案；
- 如果上面两种情况均不满足，说明 `[li,ri]`与 `S` 重叠，我们无需将 `[li,ri]`加入答案。此时，我们需要将 `S` 与 `[li,ri]`合并，即将 `S` 更新为其与 `[li,ri]`的并集。

那么我们应当在什么时候将区间 `S` 加入答案呢？由于我们需要保证答案也是按照左端点排序的，因此当我们遇到第一个 满足 `li>right`的区间时，说明以后遍历到的区间不会与 `S` 重叠，并且它们左端点一定会大于 `S` 的左端点。此时我们就可以将 `S` 加入答案。特别地，如果不存在这样的区间，我们需要在遍历结束后，将 `S` 加入答案。

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int left = newInterval[0];
        int right = newInterval[1];
        boolean placed = false;
        List<int[]> ansList = new ArrayList<int[]>();
        for (int[] interval : intervals) {
            if (interval[0] > right) {
                // 在插入区间的右侧且无交集
                if (!placed) {
                    ansList.add(new int[]{left, right});
                    placed = true;                    
                }
                ansList.add(interval);
            } else if (interval[1] < left) {
                // 在插入区间的左侧且无交集
                ansList.add(interval);
            } else {
                // 与插入区间有交集，计算它们的并集
                left = Math.min(left, interval[0]);
                right = Math.max(right, interval[1]);
            }
        }
        if (!placed) {
            ansList.add(new int[]{left, right});
        }
        int[][] ans = new int[ansList.size()][2];
        for (int i = 0; i < ansList.size(); ++i) {
            ans[i] = ansList.get(i);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是数组 `intervals` 的长度，即给定的区间个数。
- 空间复杂度：$O(1)$。除了存储返回答案的空间以外，我们只需要额外的常数空间即可。


