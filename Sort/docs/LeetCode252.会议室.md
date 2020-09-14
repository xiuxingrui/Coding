# [LeetCode252.会议室](https://leetcode-cn.com/problems/meeting-rooms/)
## 题目描述
给定一个会议时间安排的数组，每个会议时间都会包括开始和结束的时间 `[[s1,e1],[s2,e2],...]` `(si < ei)`，请你判断一个人是否能够参加这里面的全部会议。

### 示例
```
输入: [[0,30],[5,10],[15,20]]
输出: false
```
```
输入: [[7,10],[2,4]]
输出: true
```
## 题解
思路是按照开始时间对会议进行排序。接着依次遍历会议，检查它是否在下个会议开始前结束。

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        if(intervals==null||intervals.length==0||intervals[0].length==0){
            return true;
        }
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
            if(intervals[i][1]>intervals[i+1][0]){
                return false;
            }
        }
        return true;
    }
}
```
### 复杂度分析
- 时间复杂度 : $O(nlogn)$。时间复杂度由排序决定。一旦排序完成，只需要 $O(n)$ 的时间来判断交叠。
- 空间复杂度 : $O(1)$。没有使用额外空间。


