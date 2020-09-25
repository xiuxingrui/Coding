# [LeetCode1288.删除被覆盖区间](https://leetcode-cn.com/problems/remove-covered-intervals/)
## 题目描述
给你一个区间列表，请你删除列表中被其他区间所覆盖的区间。

只有当 `c <= a` 且 `b <= d` 时，我们才认为区间 `[a,b)` 被区间 `[c,d)` 覆盖。

在完成所有删除操作后，请你返回列表中剩余区间的数目。

- `1 <= intervals.length <= 1000`
- `0 <= intervals[i][0] < intervals[i][1] <= 10^5`
- 对于所有的 `i != j：intervals[i] != intervals[j]`

### 示例
```
输入：intervals = [[1,4],[3,6],[2,8]]
输出：2
解释：区间 [3,6] 被区间 [2,8] 覆盖，所以它被删除了。
```
## 题解
当区间左端点相同的时候，右端点靠后的应该放在前面。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200925153645.png)
```java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        Arrays.sort(intervals,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                if(a[0]==b[0]){
                    return b[1]-a[1];
                }else{
                    return a[0]-b[0];
                }
            }
        });
        int len=intervals.length;
        int ans=len;
        List<int[]> list=new ArrayList<>();
        for(int i=0;i<len;i++){
            if(list.isEmpty()||intervals[i][0]>list.get(list.size()-1)[1]){
                list.add(intervals[i]);
            }else{
                if(intervals[i][1]>list.get(list.size()-1)[1]){
                    list.get(list.size()-1)[1]=intervals[i][1];
                }else{
                    ans--;
                }
            }
        }
        return ans;
    }
}
```
