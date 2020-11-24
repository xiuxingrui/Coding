# [LeetCode182.查找重复的电子邮箱](https://leetcode-cn.com/problems/duplicate-emails/)
## 题目描述
编写一个 `SQL` 查询，查找 `Person` 表中所有重复的电子邮箱。

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```
根据以上输入，你的查询应返回以下结果：
```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```
说明：所有电子邮箱都是小写字母。

## 题解
```SQL
# Write your MySQL query statement below
SELECT Email 
FROM Person
GROUP BY Email
HAVING COUNT(*)>1
```
```sql
# Write your MySQL query statement below
SELECT Email 
FROM 
(
    SELECT Email,COUNT(*) AS num
    FROM Person
    GROUP BY Email
) AS temp
WHERE num>1
```
```SQL
# Write your MySQL query statement below
SELECT DISTINCT a.Email
FROM Person a,Person b
WHERE a.Email=b.Email AND a.Id!=b.Id
```