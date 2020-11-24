# [LeetCode196.删除重复的电子邮箱](https://leetcode-cn.com/problems/delete-duplicate-emails/)
## 题目描述
编写一个 `SQL` 查询，来删除 `Person` 表中所有重复的电子邮箱，重复的邮箱里只保留 `Id` 最小 的那个。

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id 是这个表的主键。
```
例如，在运行你的查询语句之后，上面的 `Person` 表应返回以下几行:
```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```
- 执行 `SQL` 之后，输出是整个 `Person` 表。
- 使用 `delete` 语句。

## 题解
```SQL
# Write your MySQL query statement below
DELETE p1 
FROM Person p1
INNER JOIN Person p2 ON p1.Email = p2.Email
WHERE p1.Id > p2.Id
```
```sql
# Write your MySQL query statement below
DELETE p1
FROM Person AS p1,Person AS p2
WHERE p1.Email = p2.Email AND p1.Id > p2.Id
```
有慢查询优化经验的同学会清楚，在实际生产中，面对千万上亿级别的数据，连接的效率往往最高，因为用到索引的概率较高。

因此，建议学习使用官方的题解，但是有两点，可能需要再解释下：

1. `DELETE p1`

在`DELETE`官方文档中，给出了这一用法，比如下面这个`DELETE`语句
```sql
DELETE t1 FROM t1 LEFT JOIN t2 ON t1.id=t2.id WHERE t2.id IS NULL;
```
这种`DELETE`方式很陌生，竟然和`SELETE`的写法类似。它涉及到`t1`和`t2`两张表，`DELETE t1`表示要删除`t1`的一些记录，具体删哪些，就看`WHERE`条件，满足就删；

这里删的是`t1`表中，跟`t2`匹配不上的那些记录。

所以，官方`sql`中，`DELETE p1`就表示从`p1`表中删除满足WHERE条件的记录。

2. `p1.Id > p2.Id`

继续之前，先简单看一下表的连接过程，这个搞懂了，理解`WHERE`条件就简单了👇

1. 从驱动表（左表）取出`N`条记录；
2. 拿着这`N`条记录，依次到被驱动表（右表）查找满足`WHERE`条件的记录；

所以，官方`sql`的过程就是

先把`Person`表搬过来
1. 从表`p1`取出3条记录；
2. 拿着第1条记录去表`p2`查找满足`WHERE`的记录，代入该条件`p1.Email = p2.Email AND p1.Id > p2.Id`后，发现没有满足的，所以不用删掉记录1；
3. 记录2同理；
4. 拿着第3条记录去表`p2`查找满足`WHERE`的记录，发现有一条记录满足，所以要从`p1`删掉记录3；
5. 3条记录遍历完，删掉了1条记录，这个`DELETE`也就结束了。

