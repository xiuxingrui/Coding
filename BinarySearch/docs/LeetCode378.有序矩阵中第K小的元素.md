# [LeetCode378.有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)
## 题目描述
给定一个 `n x n` 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 `k` 小的元素。
请注意，它是排序后的第 `k` 小元素，而不是第 `k` 个不同的元素。

你可以假设 `k` 的值永远是有效的，`1 ≤ k ≤ n^2` 。
### 示例
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。
```
## 题解
### 解法一
二分法：

由题目给出的性质可知，这个矩阵内的元素是从左上到右下递增的（假设矩阵左上角为 `matrix[0][0]`）。以下图为例：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201027002235.png)

我们知道整个二维数组中 `matrix[0][0]` 为最小值，`matrix[n−1][n−1]` 为最大值，现在我们将其分别记作 `l` 和 `r`。

可以发现一个性质：任取一个数 `mid` 满足 `l≤mid≤r`，那么矩阵中不大于 `mid` 的数，肯定全部分布在矩阵的左上角。

例如下图，取 `mid=8`：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201027002402.png)

我们可以看到，矩阵中大于 `mid` 的数就和不大于 `mid` 的数分别形成了两个板块，沿着一条锯齿线将这个矩形分开。其中左上角板块的大小即为矩阵中不大于 `mid` 的数的数量。

读者也可以自己取一些 `mid` 值，通过画图以加深理解。

我们只要沿着这条锯齿线走一遍即可计算出这两个板块的大小，也自然就统计出了这个矩阵中不大于 `mid` 的数的个数了。

走法演示如下，依然取 `mid=8`：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201027002549.png)

可以这样描述走法：

- 初始位置在 `matrix[n−1][0]`（即左下角）；
- 设当前位置为 `matrix[i][j]`。若 `matrix[i][j]≤mid`，则将当前所在列的不大于 `mid` 的数的数量（即 `i+1`）累加到答案中，并向右移动，否则向上移动；
不断移动直到走出格子为止。

我们发现这样的走法时间复杂度为 $O(n)$，即我们可以线性计算对于任意一个 `mid`，矩阵中有多少数不大于它。这满足了二分查找的性质。

不妨假设答案为 xxx，那么可以知道 l≤x≤rl\leq x\leq rl≤x≤r，这样就确定了二分查找的上下界。

每次对于「猜测」的答案 `mid`，计算矩阵中有多少数不大于 `mid` ：

- 如果数量不少于 `k`，那么说明最终答案 `x` 不大于 `mid`；
- 如果数量少于 `k`，那么说明最终答案 `x` 大于 `mid`。
这样我们就可以计算出最终的结果 `x` 了。
```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0];
        int right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (check(matrix, mid, k, n)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    public boolean check(int[][] matrix, int mid, int k, int n) {
        int i = n - 1;
        int j = 0;
        int num = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] <= mid) {
                num += i + 1;
                j++;
            } else {
                i--;
            }
        }
        return num >= k;
    }
}
```
```python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        n = len(matrix)

        def check(mid):
            i, j = n - 1, 0
            num = 0
            while i >= 0 and j < n:
                if matrix[i][j] <= mid:
                    num += i + 1
                    j += 1
                else:
                    i -= 1
            return num >= k

        left, right = matrix[0][0], matrix[-1][-1]
        while left < right:
            mid = (left + right) // 2
            if check(mid):
                right = mid
            else:
                left = mid + 1
        
        return left
```
#### 复杂度分析
- 时间复杂度：$O(nlog(r−l))$，二分查找进行次数为 $O(log(r−l))$，每次操作时间复杂度为 $O(n)$。
- 空间复杂度：$O(1)$。

### 解法二
优先队列

由题目给出的性质可知，这个矩阵的每一行均为一个有序数组。问题即转化为从这 `n` 个有序数组中找第 `k` 大的数，可以想到利用归并排序的做法，归并到第 `k` 个数即可停止。

一般归并排序是两个数组归并，而本题是 `n` 个数组归并，所以需要用小根堆维护，以优化时间复杂度。

具体如何归并，可以参考`LeetCode23.合并K个排序链表`。

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n=matrix.length;
        Queue<int[]> pqueue=new PriorityQueue<>(new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                return a[0]-b[0];
            }
        });
        for(int[] arr:matrix){
            pqueue.offer(arr);
        }
        int ans=0;
        while(k>0){
            int[] temp=pqueue.poll();
            ans=temp[0];
            if(temp.length>1){
                pqueue.offer(Arrays.copyOfRange(temp,1,temp.length));
            }
            k--;
        }
        return ans;
    }
}
```
```python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        n=len(matrix)
        heapq.heapify(matrix)
        ans=0
        for i in range(k):
            temp=heapq.heappop(matrix)
            ans=temp[0]
            if(len(temp)>1):
                heapq.heappush(matrix,temp[1:])
        return ans
```
或
```python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        pq=[]
        for arr in matrix:
            pq.append((arr[0],arr))
        heapq.heapify(pq)
        ans=0
        for i in range(k):
            item,arr=heapq.heappop(pq)
            ans=item
            if(len(arr)>1):
                heapq.heappush(pq,(arr[1],arr[1:]))
        return ans
```
#### 复杂度分析
- 时间复杂度：$O(klogn)$，归并 `k` 次，每次堆中插入和弹出的操作时间复杂度均为 `logn`。
- 空间复杂度：$O(n)$，堆的大小始终为 `n`。

### 解法三
直接排序
```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int rows = matrix.length, columns = matrix[0].length;
        int[] sorted = new int[rows * columns];
        int index = 0;
        for (int[] row : matrix) {
            for (int num : row) {
                sorted[index++] = num;
            }
        }
        Arrays.sort(sorted);
        return sorted[k - 1];
    }
}
```
```python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        rec = sorted(sum(matrix, []))# sum()执行效果是 matrix 中的子列表逐一与第二个参数相加，而列表的加法相当于 extend 操作，所以最终结果是由 [] 扩充成的列表。
        return rec[k - 1]
```
