# [LeetCode197.上升的温度](https://leetcode-cn.com/problems/rising-temperature/)
表 `Weather`
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
```
`id` 是这个表的主键
该表包含特定日期的温度信息
 

编写一个 `SQL` 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 `id` 。

返回结果 不要求顺序 。

查询结果格式如下例：

`Weather`
```
+----+------------+-------------+
| id | recordDate | Temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
```
`Result table`:
```
+----+
| id |
+----+
| 2  |
| 4  |
+----+
```
2015-01-02 的温度比前一天高（10 -> 25）
2015-01-04 的温度比前一天高（30 -> 20）
## 题解
两个时间计算的函数：

`datediff(日期1, 日期2)`：得到的结果是日期1与日期2相差的天数。

如果日期1比日期2大，结果为正；如果日期1比日期2小，结果为负。

`timestampdiff(时间类型, 日期1, 日期2)`:这个函数和上面`diffdate`的正、负号规则刚好相反。

日期1大于日期2，结果为负，日期1小于日期2，结果为正。

在“时间类型”的参数位置，通过添加`“day”`, `“hour”`, `“second”`等关键词，来规定计算天数差、小时数差、还是分钟数差。

```sql
# Write your MySQL query statement below
SELECT a.id
FROM Weather AS a
JOIN Weather AS b
WHERE DATEDIFF(a.recordDate,b.recordDate)=1 AND a.Temperature>b.Temperature;
```
或·
```sql
# Write your MySQL query statement below
SELECT a.id
FROM Weather AS a
JOIN Weather AS b ON DATEDIFF(a.recordDate, b.recordDate) = 1
WHERE a.Temperature > b.Temperature;
```
