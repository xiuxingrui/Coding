# [LeetCode185.部门工资前三高的所有员工](https://leetcode-cn.com/problems/department-top-three-salaries/)
## 题目描述
`Employee` 表包含所有员工信息，每个员工有其对应的工号 `Id`，姓名 `Name`，工资 `Salary` 和部门编号 `DepartmentId` 。

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
```
`Department` 表包含公司所有部门的信息。
```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+

```
编写一个 `SQL` 查询，找出每个部门获得前三高工资的所有员工。例如，根据上述给定的表，查询结果应返回：

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

IT 部门中，Max 获得了最高的工资，Randy 和 Joe 都拿到了第二高的工资，Will 的工资排第三。销售部门（Sales）只有两名员工，Henry 的工资最高，Sam 的工资排第二。

## 题解
### 解法一
公司里前 3 高的薪水意味着有不超过 3 个工资比这些值大。
```sql
select e1.Name as 'Employee', e1.Salary
from Employee e1
where 3 >
(
    select count(distinct e2.Salary)
    from Employee e2
    where e2.Salary > e1.Salary
)
;
```
在这个代码里，我们统计了有多少人的工资比 `e1.Salary` 高，所以样例的输出应该如下所示。
```
| Employee | Salary |
|----------|--------|
| Henry    | 80000  |
| Max      | 90000  |
| Randy    | 85000  |
```
然后，我们需要把表 `Employee` 和表 `Department` 连接来获得部门信息。

```sql
# Write your MySQL query statement below
SELECT D.Name AS Department,E1.Name AS Employee,E1.Salary AS Salary
FROM Employee AS E1
INNER JOIN Department AS D ON E1.DepartmentId=D.Id
WHERE (SELECT COUNT(DISTINCT E2.Salary) FROM Employee AS E2 WHERE E2.Salary>E1.Salary AND E1.DepartmentId=E2.DepartmentId)<3
ORDER BY D.Name,E1.Salary DESC;
```
解释：

我们先找出公司里前 3 高的薪水，意思是不超过三个值比这些值大
```sql
SELECT e1.Salary 
FROM Employee AS e1
WHERE 3 > 
		(SELECT  count(DISTINCT e2.Salary) 
		 FROM	Employee AS e2 
	 	 WHERE	e1.Salary < e2.Salary 	AND e1.DepartmentId = e2.DepartmentId) ;
```
举个栗子：

当 `e1 = e2 = [4,5,6,7,8]`

- `e1.Salary = 4，e2.Salary` 可以取值 `[5,6,7,8]`，`COUNT(DISTINCT e2.Salary) = 4`
- `e1.Salary = 5，e2.Salary` 可以取值 `[6,7,8]`，`COUNT(DISTINCT e2.Salary) = 3`
- `e1.Salary = 6，e2.Salary` 可以取值 `[7,8]`，`COUNT(DISTINCT e2.Salary) = 2`
- `e1.Salary = 7，e2.Salary` 可以取值 `[8]`，`COUNT(DISTINCT e2.Salary) = 1`
- `e1.Salary = 8，e2.Salary` 可以取值 `[]`，`COUNT(DISTINCT e2.Salary) = 0`

最后 `3 > count(DISTINCT e2.Salary)`，所以 `e1.Salary` 可取值为 `[6,7,8]`，即集合前 3 高的薪水

再把表 `Department` 和表 `Employee` 连接，获得各个部门工资前三高的员工。

### 解法二
1. 考察如何使用窗口函数及专用窗口函数排名的区别：`rank`, `dense_rank`, `row_number`
2. 经典`topN`问题：每组最大的N条记录。这类问题涉及到“既要分组，又要排序”的情况，要能想到用窗口函数来实现。

模板
```sql
# topN问题 sql模板
select *
from (
   select *, 
          row_number() over (partition by 要分组的列名
                       order by 要排序的列名 desc) as 排名
   from 表名) as a
where 排名 <= N;
```

```SQL
# Write your MySQL query statement below
SELECT D.Name AS Department,E.Name AS Employee,E.Salary AS Salary
FROM (SELECT *,dense_rank() OVER(PARTITION BY DepartmentId ORDER BY Salary DESC) AS RK FROM Employee) AS E
INNER JOIN Department AS D ON E.DepartmentId=D.Id
WHERE RK<=3
```
