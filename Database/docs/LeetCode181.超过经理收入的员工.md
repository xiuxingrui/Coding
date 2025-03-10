# [LeetCode181.超过经理收入的员工](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)
## 题目描述
`Employee` 表包含所有员工，他们的经理也属于员工。每个员工都有一个 `Id`，此外还有一列对应员工的经理的 `Id`。

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

给定 `Employee` 表，编写一个 `SQL` 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，`Joe` 是唯一一个收入超过他的经理的员工。

```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```
## 题解
### 解法一
```sql
SELECT
     a.NAME AS Employee
FROM Employee AS a JOIN Employee AS b
     ON a.ManagerId = b.Id
     AND a.Salary > b.Salary
;
```
自己的写法
```sql
# Write your MySQL query statement below
SELECT a.Name AS Employee
FROM Employee AS a
INNER JOIN Employee AS b ON a.ManagerId=b.Id
WHERE a.Salary>b.Salary;
```
### 解法二
```SQL
SELECT a.Name AS Employee
FROM Employee AS a,Employee AS b
WHERE a.ManagerId = b.Id AND a.Salary > b.Salary;
```
### 解法三
子连接：
```sql
# Write your MySQL query statement below
SELECT e.Name AS Employee
FROM Employee e
WHERE Salary > (SELECT salary FROM Employee WHERE Id = e.ManagerId)
```