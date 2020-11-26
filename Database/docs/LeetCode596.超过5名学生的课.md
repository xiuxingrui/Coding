# [LeetCode596.超过5名学生的课](https://leetcode-cn.com/problems/classes-more-than-5-students/)
## 题目描述
有一个`courses` 表 ，有: `student` (学生) 和 `class` (课程)。

请列出所有超过或等于5名学生的课。

例如，表：
```
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+
```
应该输出:
```
+---------+
| class   |
+---------+
| Math    |
+---------+
```

提示：

学生在每个课中不应被重复计算。

## 题解
### 解法一
在 `GROUP BY` 子句后使用 `HAVING` 条件是实现子查询的一种更加简单直接的方法。
```sql
SELECT
    class
FROM
    courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5
;
```
### 解法二
先统计每门课程的学生数量，再从中选择超过 5 名学生的课程。

使用 `GROUP BY` 和 `COUNT` 获得每门课程的学生数量。
```sql
SELECT
    class, COUNT(DISTINCT student)
FROM
    courses
GROUP BY class
;
```
注：使用 `DISTINCT` 防止在同一门课中学生被重复计算。

```
| class    | COUNT(student) |
|----------|----------------|
| Biology  | 1              |
| Computer | 1              |
| English  | 1              |
| Math     | 6              |
```
使用上面查询结果的临时表进行子查询，筛选学生数量超过 5 的课程。
```sql
SELECT
    class
FROM
    (SELECT
        class, COUNT(DISTINCT student) AS num
    FROM
        courses
    GROUP BY class) AS temp_table
WHERE
    num >= 5
;
```
注：`COUNT(student)` 不能直接在 `WHERE` 子句中使用，这里将其重命名为 `num`。
