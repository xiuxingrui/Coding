# [LeetCode171.Excel表列序号](https://leetcode-cn.com/problems/excel-sheet-column-number/)
## 题目描述
给定一个Excel表格中的列名称，返回其相应的列序号。

例如，
```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```
### 示例
```
输入: "A"
输出: 1
```
```
输入: "AB"
输出: 28
```
```
输入: "ZY"
输出: 701
```
## 题解
- 标签：字符串遍历，进制转换
- 初始化结果 `ans = 0`，遍历时将每个字母与 `A` 做减法，因为 `A` 表示 1，所以减法后需要每个数加 1，计算其代表的数值 `num = 字母 - ‘A’ + 1`
- 因为有 26 个字母，所以相当于 26 进制，每 26 个数则向前进一位
- 所以每遍历一位则`ans = ans * 26 + num`
- 以 `ZY` 为例，`Z` 的值为 26，`Y` 的值为 25，则结果为 26 * 26 + 25=701
```java
class Solution {
    public int titleToNumber(String s) {
        char[] arr=s.toCharArray();
        int ans=0;
        for(int i=0;i<s.length();i++){
            ans=ans*26+arr[i]-'A'+1;
        }
        return ans;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n)$
