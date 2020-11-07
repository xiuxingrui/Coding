# [LeetCode176.第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary/)
## 题目描述
编写一个 `SQL` 查询，获取 `Employee` 表中第二高的薪水（`Salary`） 。

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
例如上述 `Employee` 表，`SQL`查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 `null`。

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```
## 题解
考虑到可能有一样的值，所以使用`distinct` 进行去重。

题目要求，如果没有第二高的薪水，返回空值，所以这里用判断空值的函数（`ifnull`）函数来处理特殊情况。

`ifnull(a,b)`函数解释：
- 如果`value`不是空，结果返回`a`
- 如果`value`是空，结果返回`b`

```sql
# Write your MySQL query statement below
SELECT IFNULL(
    (
    SELECT DISTINCT Salary
    FROM Employee
    Order BY Salary DESC
    LIMIT 1 OFFSET 1),
    NULL
) AS SecondHighestSalary;
```