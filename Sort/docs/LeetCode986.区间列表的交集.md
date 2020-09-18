# [LeetCode986.区间列表的交集](https://leetcode-cn.com/problems/interval-list-intersections/)
## 题目描述
给定两个由一些 闭区间 组成的列表，每个区间列表都是成对不相交的，并且已经排序。

返回这两个区间列表的交集。

形式上，闭区间 `[a, b]`（其中 `a <= b`）表示实数 `x` 的集合，而 `a <= x <= b`。两个闭区间的交集是一组实数，要么为空集，要么为闭区间。例如，`[1, 3]` 和 `[2, 4]` 的交集为 `[2, 3]`。）

- `0 <= A.length < 1000`
- `0 <= B.length < 1000`
- `0 <= A[i].start, A[i].end, B[i].start, B[i].end < 10^9`

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200916160818.png)

```
输入：A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]
输出：[[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
```
## 题解
### 解法一
我们称 `b` 为区间 `[a, b]` 的末端点。

在两个数组给定的所有区间中，假设拥有最小末端点的区间是 `A[0]`。（为了不失一般性，该区间出现在数组 `A` 中)

然后，在数组 `B` 的区间中， `A[0]` 只可能与数组 `B` 中的至多一个区间相交。（如果 `B` 中存在两个区间均与 `A[0]` 相交，那么它们将共同包含 `A[0]` 的末端点，但是 `B` 中的区间应该是不相交的，所以存在矛盾）

如果 `A[0]` 拥有最小的末端点，那么它只可能与 `B[0]` 相交。然后我们就可以删除区间 `A[0]`，因为它不能与其他任何区间再相交了。

相似的，如果 `B[0]` 拥有最小的末端点，那么它只可能与区间 `A[0]` 相交，然后我们就可以将 `B[0]` 删除，因为它无法再与其他区间相交了。

我们用两个指针 `i` 与 `j` 来模拟完成删除 `A[0]` 或 `B[0]` 的操作。

```java
class Solution {
  public int[][] intervalIntersection(int[][] A, int[][] B) {
    List<int[]> ans = new ArrayList();
    int i = 0, j = 0;

    while (i < A.length && j < B.length) {
      // Let's check if A[i] intersects B[j].
      // lo - the startpoint of the intersection
      // hi - the endpoint of the intersection
      int lo = Math.max(A[i][0], B[j][0]);
      int hi = Math.min(A[i][1], B[j][1]);
      if (lo <= hi)
        ans.add(new int[]{lo, hi});

      // Remove the interval with the smallest endpoint
      if (A[i][1] < B[j][1])
        i++;
      else
        j++;
    }

    return ans.toArray(new int[ans.size()][]);
  }
}
```
或
```java
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {
        int na = A.length, nb = B.length;
        if (na == 0 || nb == 0) {
            return new int[0][0];
        }
        int[][] res = new int[na + nb][];
        int idxRes = 0, idxA = 0, idxB = 0;
        while (idxA < na && idxB < nb) {
            int max_left = Math.max(A[idxA][0], B[idxB][0]);
            int min_right = Math.min(A[idxA][1], B[idxB][1]);
            //得到交集，添加到结果数组
            if (max_left <= min_right) {
                res[idxRes++] = new int[]{max_left, min_right};
            }
            //右移指针
            if (A[idxA][1] < B[idxB][1]) {
                idxA++;
            } else {
                idxB++;
            }
        }
        res = Arrays.copyOf(res, idxRes);
        return res;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(M+N)$，其中 `M`,`N`分别是数组 `A` 和 `B` 的长度。
- 空间复杂度：$O(M+N)$，答案中区间数量的上限。

### 解法二
自己的解法：
```java
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {
        int m=A.length,n=B.length;
        if(m==0||n==0){
            return new int[0][2];
        }
        int[][] C=new int[m+n][2];
        int i=0,j=0,idx=0;
        int[][] temp=new int[m+n][2];
        int[][] ans=new int[m+n][2];
        while(i<m&&j<n){
            if(A[i][0]<=B[j][0]){
                C[idx++]=A[i++];
            }else if(A[i][0]>B[j][0]){
                C[idx++]=B[j++];
            }
        }
        while(i<m){
            C[idx++]=A[i++];
        }
        while(j<n){
            C[idx++]=B[j++];
        }
        idx=-1;
        int index=0;
        for(int k=0;k<m+n;k++){
            if(idx!=-1&&C[k][0]<=temp[idx][1]){
                ans[index++]=new int[]{C[k][0],Math.min(C[k][1],temp[idx][1])};
            }
            if(idx==-1||C[k][0]>temp[idx][1]){
                temp[++idx]=C[k];
            }else{
                temp[idx][1]=Math.max(C[k][1],temp[idx][1]);
            }
        }
        return Arrays.copyOf(ans,index);
    }
}
```
