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
