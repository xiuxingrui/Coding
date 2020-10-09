# [剑指Offer50.第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)
## 题目描述
在字符串 `s` 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 `s` 只包含小写字母。

- `0 <= s 的长度 <= 50000`
### 示例
```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```
## 题解
哈希表
```java
class Solution {
    public char firstUniqChar(String s) {
        int[] count = new int[256];
        char[] chars = s.toCharArray();
        for(char c : chars)
            count[c]++;
        for(char c : chars){
            if(count[c] == 1)
                return c;
        }
        return ' ';
    }
}
```
或
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
有序哈希表：

在哈希表的基础上，有序哈希表中的键值对是 按照插入顺序排序 的。基于此，可通过遍历有序哈希表，实现搜索首个 “数量为 1的字符”。

哈希表是 去重 的，即哈希表中键值对数量`≤`字符串 `s` 的长度。因此，相比于前面的方法，该方法减少了第二轮遍历的循环次数。当字符串很长（重复字符很多）时，该方法则效率更高。
```java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character, Integer> map= new LinkedHashMap<>();
        char[] sc = s.toCharArray();
        for(char c : sc){
            map.put(c,map.getOrDefault(c,0)+1);
        }
        for(Map.Entry<Character,Integer> d : map.entrySet()){
           if(d.getValue()==1) return d.getKey();
        }
        return ' ';
    }
}
```