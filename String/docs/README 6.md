# [剑指Offer50.第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)
##  题目描述
在字符串 `s` 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 `s` 只包含小写字母。

### 示例
```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```
## 题解
```java
class Solution {
    public char firstUniqChar(String s) {
        HashMap<Character,Integer> map=new HashMap<>();
        char ans=' ';
        char[] charArray=s.toCharArray();
        for(char c:charArray){
            map.put(c,map.getOrDefault(c,0)+1);
        }
        for(char c:charArray){
            if(map.get(c)==1){
                ans=c;
                break;
            }
        }
        return ans;
    }
}
```