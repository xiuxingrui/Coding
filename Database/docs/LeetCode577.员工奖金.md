# [LeetCode577.员工奖金](https://leetcode-cn.com/problems/employee-bonus/)
## 题目描述
选出所有 `bonus < 1000` 的员工的 `name` 及其 `bonus`。

```
Employee 表单

+-------+--------+-----------+--------+
| empId |  name  | supervisor| salary |
+-------+--------+-----------+--------+
|   1   | John   |  3        | 1000   |
|   2   | Dan    |  3        | 2000   |
|   3   | Brad   |  null     | 4000   |
|   4   | Thomas |  3        | 4000   |
+-------+--------+-----------+--------+
empId 是这张表单的主关键字
```
```
Bonus 表单

+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
empId 是这张表单的主关键字
```

输出示例：
```
+-------+-------+
| name  | bonus |
+-------+-------+
| John  | null  |
| Dan   | 500   |
| Brad  | null  |
+-------+-------+
```
## 题解
将表 `Employee` 和 `Bonus` 连接，然后使用 `WHERE` 语句获得想要的记录。

由于外键 `Bonus.empId` 引用了 `Employee.empId` 且有一些雇员没有 `bonus` 记录，所以第一步中，我们使用 `OUTER JOIN` 将两个表连接。

注意： `"LEFT OUTER JOIN"` 可以被写作 `"LEFT JOIN"` 。

`Brad` 和 `John` 的 `bonus` 值为空，在数据库中会被记为 `NULL`。从概念上来说，`NULL` 意味着 “一个缺失的未知的值” 。除此以外，我们使用 `IS NULL` 和 `IS NOT NULL` （而不是 `bonus = null`）来将一个值与 `NULL` 做比较。

最后，我们增加一个 `WHERE` 语句以筛选出我们想要的记录。

```sql
# Write your MySQL query statement below
SELECT name,bonus
FROM Employee
LEFT OUTER JOIN Bonus ON Employee.empID=Bonus.empID
WHERE bonus IS NULL OR bonus < 1000 ;
```