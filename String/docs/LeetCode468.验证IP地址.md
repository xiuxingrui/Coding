# [LeetCode468.验证IP地址](https://leetcode-cn.com/problems/validate-ip-address/)
## 题目描述
编写一个函数来验证输入的字符串是否是有效的 `IPv4` 或 `IPv6` 地址。

- 如果是有效的 `IPv4` 地址，返回 `"IPv4"` ；
- 如果是有效的 `IPv6` 地址，返回 `"IPv6"` ；
- 如果不是上述类型的 `IP` 地址，返回 `"Neither"` 。
`IPv4` 地址由十进制数和点来表示，每个地址包含 4 个十进制数，其范围为 0 - 255， 用`(".")`分割。比如，`172.16.254.1`；

同时，`IPv4` 地址内的数不会以 0 开头。比如，地址 `172.16.254.01` 是不合法的。

`IPv6` 地址由 8 组 16 进制的数字来表示，每组表示 16 比特。这些组数字通过 `(":")`分割。比如, `2001:0db8:85a3:0000:0000:8a2e:0370:7334` 是一个有效的地址。而且，我们可以加入一些以 0 开头的数字，字母可以使用大写，也可以是小写。所以， `2001:db8:85a3:0:0:8A2E:0370:7334` 也是一个有效的 `IPv6 address`地址 (即，忽略 0 开头，忽略大小写)。

然而，我们不能因为某个组的值为 0，而使用一个空的组，以至于出现 `(::)` 的情况。 比如， `2001:0db8:85a3::8A2E:0370:7334` 是无效的 `IPv6` 地址。

同时，在 `IPv6` 地址中，多余的 0 也是不被允许的。比如， `02001:0db8:85a3:0000:0000:8a2e:0370:7334` 是无效的。

- `IP` 仅由英文字母，数字，字符 `'.'` 和 `':'` 组成。
## 示例
```
输入：IP = "172.16.254.1"
输出："IPv4"
解释：有效的 IPv4 地址，返回 "IPv4"
```
```
输入：IP = "2001:0db8:85a3:0:0:8A2E:0370:7334"
输出："IPv6"
解释：有效的 IPv6 地址，返回 "IPv6"
```
```
输入：IP = "256.256.256.256"
输出："Neither"
解释：既不是 IPv4 地址，又不是 IPv6 地址
```
```
输入：IP = "2001:0db8:85a3:0:0:8A2E:0370:7334:"
输出："Neither"
```
```
输入：IP = "1e1.4.5.6"
输出："Neither"
```
## 题解
对于 `IPv4` 地址，通过界定符 `.` 将地址分为四块；对于 `IPv6` 地址，通过界定符 `:` 将地址分为八块。

对于 `IPv4` 地址的每一块，检查它们是否在 `0 - 255` 内，且没有前置零。

对于 `IPv6` 地址的每一块，检查其长度是否为 `1 - 4` 位的十六进制数。

```java
class Solution {
    public String validIPAddress(String IP) {
        if(IP==null||IP.length()==0){
            return "Neither";
        }
        int len=IP.length();
        char[] arr=IP.toCharArray();
        int idx=0;
        while(idx<len&&arr[idx]!='.'&&arr[idx]!=':'){
            idx++;
        }
        if(idx==len){
            return "Neither";
        }
        if(arr[len-1]=='.'||arr[len-1]==':'){
            return "Neither";
        }
        if(arr[idx]=='.'){
            String[] temp=IP.split("\\.");
            if(temp.length!=4){
                return "Neither";
            }
            for(int i=0;i<4;i++){
                int sum=0;
                char[] s=temp[i].toCharArray();
                if(s.length==0||s.length!=1&&s[0]=='0'){
                    return "Neither";
                }
                for(int j=0;j<s.length;j++){
                    if(!(s[j]>='0'&&s[j]<='9')){
                        return "Neither";
                    }
                    sum=sum*10+s[j]-'0';
                    if(sum>255){
                        return "Neither";
                    }
                }
            }
            return "IPv4";
        }
        if(arr[idx]==':'){
            String[] temp=IP.split(":");
            if(temp.length!=8){
                return "Neither";
            }
            for(int i=0;i<8;i++){
                char[] s=temp[i].toCharArray();
                if(s.length==0){
                    return "Neither";
                }
                if(s.length>4){
                    return "Neither";
                }
                for(int j=0;j<s.length;j++){
                    if(!(s[j]>='0'&&s[j]<='9'||s[j]>='a'&&s[j]<='f'||s[j]>='A'&&s[j]<='F')){
                        return "Neither";
                    }
                }
            }
            return "IPv6";
        }
        return "Neither";
    }
}
```