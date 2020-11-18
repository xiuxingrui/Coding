# [LeetCode177.第N高的薪水](https://leetcode-cn.com/problems/nth-highest-salary/)
## 题目描述
编写一个 `SQL` 查询，获取 `Employee` 表中第 `n` 高的薪水（`Salary`）。

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
例如上述 `Employee` 表，`n = 2` 时，应返回第二高的薪水 200。如果不存在第 `n` 高的薪水，那么查询应返回 `null`。

```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```
## 题解
`MySQL`查询的一般性思路是：

- 能用单表优先用单表，即便是需要用`group by`、`order by`、`limit`等，效率一般也比多表高
- 不能用单表时优先用连接，连接是`SQL`中非常强大的用法，小表驱动大表+建立合适索引+合理运用连接条件，基本上连接可以解决绝大部分问题。但`join`级数不宜过多，毕竟是一个接近指数级增长的关联效果
- 能不用子查询、笛卡尔积尽量不用，虽然很多情况下`MySQL`优化器会将其优化成连接方式的执行过程，但效率仍然难以保证
- 自定义变量在复杂`SQL`实现中会很有用，例如`LeetCode`中困难级别的数据库题目很多都需要借助自定义变量实现
- 如果`MySQL`版本允许，某些带聚合功能的查询需求应用窗口函数是一个最优选择。除了经典的获取3种排名信息，还有聚合函数、向前向后取值、百分位等，具体可参考官方指南。以下是官方给出的几个窗口函数的介绍：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201117231529.png)

最后的最后再补充一点，本题将查询语句封装成一个自定义函数并给出了模板，实际上是降低了对函数语法的书写要求和难度，而且提供的函数写法也较为精简。然而，自定义函数更一般化和常用的写法应该是分三步：

- 定义变量接收返回值
- 执行查询条件，并赋值给相应变量
- 返回结果
### 解法一
窗口函数：

实际上，在`mysql8.0`中有相关的内置函数，而且考虑了各种排名问题：

- `row_number()`: 同薪不同名，相当于行号，例如3000、2000、2000、1000排名后为1、2、3、4
- `rank()`: 同薪同名，有跳级，例如3000、2000、2000、1000排名后为1、2、2、4
- `dense_rank()`: 同薪同名，无跳级，例如3000、2000、2000、1000排名后为1、2、2、3
- `ntile()`: 分桶排名，即首先按桶的个数分出第一二三桶，然后各桶内从1排名，实际不是很常用
显然，本题是要用第三个函数。

另外这三个函数必须要要与其搭档`over()`配套使用，`over()`中的参数常见的有两个，分别是

- `partition by`，按某字段切分
- `order by`，与常规`order by`用法一致，也区分`ASC`(默认)和`DESC`，因为排名总得有个依据

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      # Write your MySQL query statement below.
    SELECT DISTINCT Salary
    FROM (SELECT Salary,dense_rank() OVER (ORDER BY Salary DESC) AS RK FROM Employee) AS tmp
    WHERE RK = N
  );
END
```
### 解法二
单表查询：

由于本题不存在分组排序，只需返回全局第`N`高的一个，所以自然想到的想法是用`ORDER BY`排序加`LIMIT`限制得到。

排名第`N`高意味着要跳过`N-1`个薪水，由于无法直接用`LIMIT N-1`，所以需先在函数开头处理`N`为`N=N-1`。

注：这里不能直接用`LIMIT N-1`是因为`LIMIT`和`OFFSET`字段后面只接受正整数（意味着0、负数、小数都不行）或者单一变量（意味着不能用表达式），也就是说想取一条，`LIMIT 2-1`、`LIMIT 1.1`这类的写法都是报错的。

`LIMIT 5`指示`MySQL`返回不超过 5 行的数据,`LIMIT 5 OFFSET 5` 指示 `MySQL`返回从第 5 行起的 5 行数据。第一个数字是检索的行数，第二个数字开始下标。

第一个被检索的行是第 0 行，而不是第 1 行。因此，`LIMIT 1 OFFSET 1` 会检索第 2 行，而不是第 1 行。

可以把 `LIMIT 4 OFFSET 3` 语句简化为 `LIMIT 3,4`。使用这个语法，逗号之前的值对应 `OFFSET`，逗号之后的值对应`LIMIT`（反着的，要小心）。


```SQL
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N-1;# 要在RETURN之上
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary# 要用DISTINCT去重
      FROM Employee
      ORDER BY Salary DESC
      LIMIT 1 OFFSET N
  );
END
```
或
```SQL
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N-1;# 要在RETURN之上
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary# 要用DISTINCT去重
      FROM Employee
      ORDER BY Salary DESC
      LIMIT N,1
  );
END
```
### 解法三
子查询：

1. 排名第`N`的薪水意味着该表中存在`N-1`个比其更高的薪水
2. 注意这里的`N-1`个更高的薪水是指去重后的`N-1`个，实际对应人数可能不止`N-1`个
3. 最后返回的薪水也应该去重，因为可能不止一个薪水排名第`N`
4. 由于对于每个薪水的`WHERE`条件都要执行一遍子查询，注定其效率低下

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT e.Salary
      FROM Employee AS e
      WHERE (SELECT COUNT(DISTINCT Salary) FROM Employee WHERE Salary>e.Salary)=N-1
  );
END
```
### 解法四
一般来说，能用子查询解决的问题也能用连接解决。具体到本题：

- 两表自连接，连接条件设定为表1的`Salary`小于表2的`Salary`
- 以表1的`Salary`分组，统计表1中每个`Salary`分组后对应表2中`Salary`唯一值个数，即去重
- 限定步骤2中`HAVING` 计数个数为`N-1`，即实现了该分组中表1`Salary`排名为第`N`个
- 考虑`N=1`的特殊情形(特殊是因为`N-1=0`，计数要求为0)，此时不存在满足条件的记录数，但仍需返回结果，所以连接用`LEFT JOIN`
如果仅查询薪水这一项值，那么不用`LEFT JOIN`当然也是可以的，只需把连接条件放宽至小于等于、同时查询个数设置为`N`即可。因为连接条件含等号，所以一定不为空，用`JOIN`即可。

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      # Write your MySQL query statement below.
      SELECT 
          e1.Salary
      FROM 
        Employee AS e1 JOIN Employee AS e2 ON e1.Salary <= e2.Salary
      GROUP BY 
          e1.Salary
      HAVING 
          count(DISTINCT e2.Salary) = N
  );
END
```
```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      # Write your MySQL query statement below.
      SELECT 
          e1.Salary
      FROM 
          Employee e1 LEFT JOIN Employee e2 ON e1.Salary < e2.Salary
      GROUP BY 
          e1.Salary
      HAVING 
          count(DISTINCT e2.Salary) = N-1
  );
END
```