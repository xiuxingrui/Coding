# [LeetCode1179.重新格式化部门表](https://leetcode-cn.com/problems/reformat-department-table/)
## 题目描述
部门表 `Department`：

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+
(id, month) 是表的联合主键。
这个表格有关于每个部门每月收入的信息。
月份（month）可以取下列值 ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"]。
```
编写一个 `SQL` 查询来重新格式化表，使得新的表中有一个部门 `id` 列和一些对应 每个月 的收入（`revenue`）列。

`Department` 表：
```
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+
```
查询得到的结果表：
```
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
```
注意，结果表有 13 列 (1个部门 id 列 + 12个月份的收入列)。

## 题解
`GROUP BY id` 会使`department`表按照`id`分组，生成一张虚拟表（假想中的表）,在虚拟表中，所有`id=1`的`revenue`或者`month`数据都写在了同一个单元格中，如8000、7000、6000都是写在同一单元格内的。真正的表是不能这样写的，所以这种写法只存在于虚拟表中，帮助我们理解。

当一个单元格中有多个数据时，`case when`只会提取当中的第一个数据。

以`CASE WHEN month='Feb' THEN revenue END` 为例，当`id=1`时，它只会提取`month`对应单元格里的第一个数据，即`Jan`，它不等于`Feb`，所以找不到`Feb`对应的`revenue`，所以返回`NULL`。（可以试试把我上面答案里的`sum()`统统去掉，执行结果与预期不一样。错就错在当`id=1`时,`Feb_Revenue`和`Mar_Revenue`的值变成了`NULL`）

那该如何解决单元格内含多个数据的情况呢？答案就是使用聚合函数，聚合函数就用来输入多个数据，输出一个数据的。如`SUM()`或`MAX()`，而每个聚合函数的输入就是每一个多数据的单元格。

以`SUM(CASE WHEN month='Feb' THEN revenue END)` 为例，当`id=1`时，它提取的`Jan`、`Feb`、`Mar`，从中找到了符合条件的`Feb`，并最终返回对应的`revenue`的值，即7000。


```sql
# Write your MySQL query statement below
SELECT id, 
SUM(CASE WHEN month='Jan' THEN revenue END) AS Jan_Revenue,
SUM(CASE WHEN month='Feb' THEN revenue END) AS Feb_Revenue,
SUM(CASE WHEN month='Mar' THEN revenue END) AS Mar_Revenue,
SUM(CASE WHEN month='Apr' THEN revenue END) AS Apr_Revenue,
SUM(CASE WHEN month='May' THEN revenue END) AS May_Revenue,
SUM(CASE WHEN month='Jun' THEN revenue END) AS Jun_Revenue,
SUM(CASE WHEN month='Jul' THEN revenue END) AS Jul_Revenue,
SUM(CASE WHEN month='Aug' THEN revenue END) AS Aug_Revenue,
SUM(CASE WHEN month='Sep' THEN revenue END) AS Sep_Revenue,
SUM(CASE WHEN month='Oct' THEN revenue END) AS Oct_Revenue,
SUM(CASE WHEN month='Nov' THEN revenue END) AS Nov_Revenue,
SUM(CASE WHEN month='Dec' THEN revenue END) AS Dec_Revenue
FROM department
GROUP BY id
ORDER BY id;
```
