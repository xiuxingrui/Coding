# [LeetCode1344.时钟指针的夹角](https://leetcode-cn.com/problems/angle-between-hands-of-a-clock/)
## 题目描述
给你两个数 `hour` 和 `minutes` 。请你返回在时钟上，由给定时间的时针和分针组成的较小角的角度（60 单位制）。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162339.png)
```
输入：hour = 12, minutes = 30
输出：165
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162350.png)
```
输入：hour = 3, minutes = 15
输出：7.5
```
```
输入：hour = 4, minutes = 50
输出：155
```
```
输入：hour = 12, minutes = 0
输出：0
```
## 题解
其思想是分别计算 0 点垂线与每个指针之间的角度。答案是这两个角度的差。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162451.png)

分针的角度：

我们从分针开始，整个圆 `360°` 有 60 分钟。分针指针移动一分钟的角度是 `1 min=360°/60=6`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162530.png)

现在可以很容易地找到 0 点垂直线和分钟指针之间的角度：`minutes_angle=minutes×6°`.

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162600.png)

时针的角度：
与分针的角度相似，整个圆 `360°` 有 12 个小时，因此每个小时 `1h=360°/12=30`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162635.png)

则时针的角度为：`hour_angle=hour×30°`。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162659.png)

由于 12 点的角度实际为 0，则需要修改表达式为：`hour_angle=(hour mod 12) ×30°`.

在分钟指针大于 0 的情况下，必须考虑到时针指针额外的移动：它不在整数值之间跳跃，是跟着分针移动。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200812162736.png)

1. 初始化常数：`one_min_angle = 6，one_hour_angle = 30`。
2. 分针指针与 0 点垂线的角度为：`minutes_angle = one_min_angle * minutes`。
3. 时针指针与 0 点垂线的角度为：`hour_angle = (hour % 12 + minutes / 60) * one_hour_angle`。
4. 得到差：`diff = abs(hour_angle - minutes_angle)`。
5. 返回最小的角度：`min(diff, 360 - diff)`。

```java
class Solution {
  public double angleClock(int hour, int minutes) {
    int oneMinAngle = 6;
    int oneHourAngle = 30;

    double minutesAngle = oneMinAngle * minutes;
    double hourAngle = (hour % 12 + minutes / 60.0) * oneHourAngle;

    double diff = Math.abs(hourAngle - minutesAngle);
    return Math.min(diff, 360 - diff);
  }
}
```
### 复杂度分析
- 时间复杂度：$O(1)$。
- 空间复杂度：$O(1)$。

