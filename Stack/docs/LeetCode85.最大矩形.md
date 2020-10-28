# [LeetCode85.最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)
## 题目描述
给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

### 示例
```
输入:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
输出: 6
```
## 题解
### 解法一
单调栈：

借鉴柱状图中的最大矩形：
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028183427.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028183414.png)

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return 0;
        }
        int m=matrix.length,n=matrix[0].length;
        int[] heights=new int[n];
        int ans=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                heights[j]=matrix[i][j]=='1'?heights[j]+1:0;
            }
            ans=Math.max(ans,getMaxArea(heights));
        }
        return ans;
    }
    public int getMaxArea(int[] heights){
        int len=heights.length;
        int[] left=new int[len];
        int[] right=new int[len];
        Arrays.fill(right,len);
        Deque<Integer> stack=new LinkedList<>();
        for(int i=0;i<len;i++){
            while(!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                right[stack.pop()]=i;
            }
            if(stack.isEmpty()){
                left[i]=-1;
            }else{
                left[i]=stack.peek();
            }
            stack.push(i);
        }
        int ans=0;
        for(int i=0;i<len;i++){
            ans=Math.max(ans,(right[i]-left[i]-1)*heights[i]);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度 : $O(NM)$。对每一行运行 力扣 84 需要 `M` (每行长度) 时间，运行了 `N` 次，共计 $O(NM)$。
- 空间复杂度 : $O(M)$。我们声明了长度等于列数的数组，用于存储每一行的宽度。

### 解法二
遍历每个点，求以这个点为矩阵右下角的所有矩阵面积。如下图的两个例子，橙色是当前遍历的点，然后虚线框圈出的矩阵是其中一个矩阵。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028155608.png)

怎么找出这样的矩阵呢？如下图，如果我们知道了以这个点结尾的连续 1 的个数的话，问题就变得简单了。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028155628.png)

首先求出高度是 1 的矩形面积，也就是它自身的数，如图中橙色的 4，面积就是 4。

然后向上扩展一行，高度增加一，选出当前列最小的数字，作为矩阵的宽，求出面积，对应上图的矩形框。

然后继续向上扩展，重复步骤 2。

按照上边的方法，遍历所有的点，求出所有的矩阵就可以了。

以橙色的点为右下角，高度为 1。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028155657.png)

高度为 2。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028155709.png)

高度为 3。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201028155721.png)

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix.length == 0) {
            return 0;
        }
        //保存以当前数字结尾的连续 1 的个数
        int[][] width = new int[matrix.length][matrix[0].length];
        int maxArea = 0;
        //遍历每一行
        for (int row = 0; row < matrix.length; row++) {
            for (int col = 0; col < matrix[0].length; col++) {
                //更新 width
                if (matrix[row][col] == '1') {
                    if (col == 0) {
                        width[row][col] = 1;
                    } else {
                        width[row][col] = width[row][col - 1] + 1;
                    }
                } else {
                    width[row][col] = 0;
                }
                //记录所有行中最小的数
                int minWidth = width[row][col];
                //向上扩展行
                for (int up_row = row; up_row >= 0; up_row--) {
                    int height = row - up_row + 1;
                    //找最小的数作为矩阵的宽
                    minWidth = Math.min(minWidth, width[up_row][col]);
                    //更新面积
                    maxArea = Math.max(maxArea, height * minWidth);
                }
            }
        }
        return maxArea;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(m^2*n)$。
- 空间复杂度：$O(m^2*n)$。
