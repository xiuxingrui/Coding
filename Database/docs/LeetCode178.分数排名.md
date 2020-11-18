# [LeetCode178.分数排名](https://leetcode-cn.com/problems/rank-scores/)
## 题目描述
编写一个 `SQL` 查询来实现分数排名。

如果两个分数相同，则两个分数排名（`Rank`）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。

```
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```
例如，根据上述给定的 `Scores` 表，你的查询应该返回（按分数从高到低排列）：

```
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

重要提示：对于 `MySQL` 解决方案，如果要转义用作列名的保留字，可以在关键字之前和之后使用撇号。例如 `Rank`

## 题解
### 解法一
1. 考察如何使用窗口函数
2. 专用窗口函数排名的区别：`rank()`,`dense_rank()`, `row_number()`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201117160125.png)
```SQL
SELECT Score,dense_rank() OVER(
    ORDER BY Score DESC
)AS `Rank`
FROM Scores;
```
### 解法二
最后的结果包含两个部分，第一部分是降序排列的分数，第二部分是每个分数对应的排名。

第一部分不难写：
```sql
SELECT a.Score AS Score
FROM Scores a
ORDER BY a.Score DESC
```
比较难的是第二部分。假设现在给你一个分数`X`，如何算出它的排名`Rank`呢？

我们可以先提取出大于等于`X`的所有分数集合`H`，将`H`去重后的元素个数就是`X`的排名。比如你考了99分，但最高的就只有99分，那么去重之后集合`H`里就只有99一个元素，个数为1，因此你的`Rank`为1。

先提取集合`H`：
```SQL
SELECT b.Score FROM Scores b WHERE   b.Score >= X;
```
我们要的是集合`H`去重之后的元素个数，因此升级为：
```sql
SELECT COUNT(DISTINCT b.Score) FROM Scores b WHERE b.Score >= X as Rank;
```
而从结果的角度来看，第二部分的`Rank`是对应第一部分的分数来的，所以这里的`X`就是上面的`a.Score`，把两部分结合在一起为：
```SQL
SELECT Score,(SELECT COUNT(DISTINCT Score) FROM Scores b WHERE b.Score>=a.Score) AS `Rank`
FROM Scores a
ORDER BY Score DESC
```


