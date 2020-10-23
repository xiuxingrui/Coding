# [LeetCode223.矩形面积](https://leetcode-cn.com/problems/rectangle-area/)
## 题目描述
在二维平面上计算出两个由直线构成的矩形重叠后形成的总面积。

每个矩形由其左下顶点和右上顶点坐标表示，如图所示。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201023135540.png)

说明: 假设矩形面积不会超出 `int` 的范围。
### 示例
```
输入: -3, 0, 3, 4, 0, -1, 9, 2
输出: 45
```
## 题解
把两个矩形叫做 A 和 B，不重叠就有四种情况，A 在 B 左边，A 在 B 右边，A 在 B 上边，A 在 B 下边。

判断上边的四种情况也很简单，比如判断 A 是否在 B 左边，只需要判断 A 的最右边的坐标是否小于 B 的最左边的坐标即可。其他情况类似。

此时矩形覆盖的面积就是两个矩形的面积和。

接下来考虑有重叠的情况。

此时我们只要求出重叠形成的矩形的面积，然后用两个矩形的面积减去重叠矩形的面积就是两个矩形覆盖的面积了。

而求重叠矩形的面积也很简单，我们只需要确认重叠矩形的四条边即可，可以结合题目的图想。

左边只需选择两个矩形的两条左边靠右的那条。

上边只需选择两个矩形的两条上边靠下的那条。

右边只需选择两个矩形的两条右边靠左的那条。

下边只需选择两个矩形的两条下边靠上的那条。

确定以后，重叠的矩形的面积也就可以算出来了。

```java
class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        //求第一个矩形的面积
        int length1 = C - A;
        int width1 = D - B;
        int area1 = length1 * width1;
        
        //求第二个矩形的面积
        int length2 = G - E;
        int width2 = H - F;
        int area2 = length2 * width2;

        // 没有重叠的情况
        if (E >= C || G <= A || F >= D || H <= B) {
            return area1 + area2;
        }
        
        //确定右边
        int x1 = Math.min(C, G);
        //确定左边
        int x2 = Math.max(E, A);
        int length3 = x1 - x2;

        //确定上边
        int y1 = Math.min(D, H);
        //确定下边
        int y2 = Math.max(F, B);
        int width3 = y1 - y2;
        int area3 = length3 * width3;

        return area1 + area2 - area3;
    }
}
```