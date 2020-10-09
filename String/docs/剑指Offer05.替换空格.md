# [剑指Offer05.替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)
## 题目描述
请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

- `0 <= s 的长度 <= 10000`
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
        char[] arr=s.toCharArray();
        StringBuilder sb=new StringBuilder();
        for(char c:arr){
            if(c==' '){
                sb.append("%20");
            }else{
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```
或
```java
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size);
        return newStr;
    }
}
```
#### 复杂度分析：
- 时间复杂度 $O(N)$ ： 遍历使用 $O(N)$，每轮添加（修改）字符操作使用 $O(1)$；
- 空间复杂度 $O(N)$： `Java` 新建的 `StringBuilder` 使用了线性大小的额外空间。

### 解法二
```java
class Solution {
    public String replaceSpace(String s) {
        return s.replace(" ","%20");
    }
}
```