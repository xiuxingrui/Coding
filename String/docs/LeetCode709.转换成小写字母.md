# [LeetCode709.转换成小写字母](https://leetcode-cn.com/problems/to-lower-case/)
## 题目描述
实现函数 `ToLowerCase()`，该函数接收一个字符串参数 `str`，并将该字符串中的大写字母转换成小写字母，之后返回新的字符串。

### 示例
```
输入: "Hello"
输出: "hello"
```
```
输入: "here"
输出: "here"
```
```
输入: "LOVELY"
输出: "lovely"
```
## 题解
```java
class Solution {
    public String toLowerCase(String str) {
        char[] arr = str.toCharArray();
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] >= 'A' && arr[i] <= 'Z') {
                arr[i] += 'a' - 'A';
            }
        }
        return String.valueOf(arr);
    }
}
```