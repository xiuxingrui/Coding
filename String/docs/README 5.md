# [剑指Offer05.替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)
## 题目描述
请实现一个函数，把字符串 `s` 中的每个空格替换成`"%20"`。

### 示例
```
输入：s = "We are happy."
输出："We%20are%20happy."
```
## 题解
### 解法一
```java
class Solution {
    public String replaceSpace(String s) {
        return s.replace(" ","%20");
    }
}
```