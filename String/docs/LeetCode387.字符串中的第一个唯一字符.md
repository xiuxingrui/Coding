# [LeetCode387.字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)
## 题目描述
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

你可以假定该字符串只包含小写字母。
### 示例
```
s = "leetcode"
返回 0
```
```
s = "loveleetcode"
返回 2
```
## 题解
这道题最优的解法就是线性复杂度了，为了保证每个元素是唯一的，至少得把每个字符都遍历一遍。

算法的思路就是遍历一遍字符串，然后把字符串中每个字符出现的次数保存在一个散列表中。这个过程的时间复杂度为 $O(N)$，其中 `N` 为字符串的长度。

接下来需要再遍历一次字符串，这一次利用散列表来检查遍历的每个字符是不是唯一的。如果当前字符唯一，直接返回当前下标就可以了。第二次遍历的时间复杂度也是 $O(N)$。

```java
class Solution {
    public int firstUniqChar(String s) {
        int[] hash=new int[26];
        char[] arr=s.toCharArray();
        for(char c:arr){
            hash[c-'a']++;
        }
        for(int i=0;i<s.length();i++){
            if(hash[arr[i]-'a']==1){
                return i;
            }
        }
        return -1;
    }
}
```
### 复杂度分析
- 时间复杂度： $O(N)$,只遍历了两遍字符串，同时散列表中查找操作是常数时间复杂度的。
- 空间复杂度： $O(N)$,用到了散列表来存储字符串中每个元素出现的次数。
