# [LeetCode175.组合两个表](https://leetcode-cn.com/problems/combine-two-tables/)
## 题目描述
```
表1: Person

+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
```
```
表2: Address

+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键
```

编写一个 `SQL` 查询，满足条件：无论 `person` 是否有地址信息，都需要基于上述两表提供 `person` 的以下信息：

`FirstName, LastName, City, State`

## 题解
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201107145324.png)

因为表 `Address` 中的 `personId` 是表 `Person` 的外关键字，所以我们可以连接这两个表来获取一个人的地址信息。

考虑到可能不是每个人都有地址信息，我们应该使用 `outer join` 而不是默认的 `inner join`。

注意：如果没有某个人的地址信息，使用 `where` 子句过滤记录将失败，因为它不会显示姓名信息。

```SQL
# Write your MySQL query statement below
SELECT FirstName,LastName,City,State
FROM Person
LEFT OUTER JOIN Address ON Address.PersonId=Person.PersonId;
```
或
```SQL
# Write your MySQL query statement below
SELECT FirstName,LastName,City,State
FROM Address
RIGHT OUTER JOIN Person ON Address.PersonId=Person.PersonId;
```

