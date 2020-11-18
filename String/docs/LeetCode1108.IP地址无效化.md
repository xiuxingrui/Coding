# [LeetCode1108.IP地址无效化](https://leetcode-cn.com/problems/defanging-an-ip-address/)
## 题目描述
给你一个有效的 `IPv4` 地址 `address`，返回这个 `IP` 地址的无效化版本。

所谓无效化 `IP` 地址，其实就是用 `"[.]"` 代替了每个 `"."`。

给出的 `address` 是一个有效的 `IPv4` 地址

### 示例
```
输入：address = "1.1.1.1"
输出："1[.]1[.]1[.]1"
```
```
输入：address = "255.100.50.0"
输出："255[.]100[.]50[.]0"
```
## 题解
### 解法一
```java
class Solution {
    public String defangIPaddr(String address) {
        return address.replace(".","[.]");   
    }
}
```
### 解法二
```java
class Solution {
    public String defangIPaddr(String address) {
        char[] arr=address.toCharArray();
        StringBuilder sb=new StringBuilder();
        int len=address.length(),i=0;
        while(i<len){
            if(arr[i]=='.'){
                //sb.append('[').append('.').append(']');
                sb.append("[.]");
            }else{
                sb.append(arr[i]);
            }
            i++;
        }
        return sb.toString();
    }
}
```

