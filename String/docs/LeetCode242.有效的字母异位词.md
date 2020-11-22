# [LeetCode242.有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)
## 题目描述
给定两个字符串 `s` 和 `t` ，编写一个函数来判断 `t` 是否是 `s` 的字母异位词。

说明:你可以假设字符串只包含小写字母。
### 示例
```
输入: s = "anagram", t = "nagaram"
输出: true
```
```
输入: s = "rat", t = "car"
输出: false
```
## 题解
为了检查 `t` 是否是 `s` 的重新排列，我们可以计算两个字符串中每个字母的出现次数并进行比较。因为 `S` 和 `T` 都只包含 `A−Z` 的字母，所以一个简单的 26 位计数器表就足够了。

我们需要两个计数器数表进行比较吗？实际上不是，因为我们可以用一个计数器表计算 `s` 字母的频率，用 `t` 减少计数器表中的每个字母的计数器，然后检查计数器是否回到零。

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] counter = new int[26];
    for (int i = 0; i < s.length(); i++) {
        counter[s.charAt(i) - 'a']++;
        counter[t.charAt(i) - 'a']--;
    }
    for (int count : counter) {
        if (count != 0) {
            return false;
        }
    }
    return true;
}
```
或者我们可以先用计数器表计算 `s`，然后用 `t` 减少计数器表中的每个字母的计数器。如果在任何时候计数器低于零，我们知道 `t` 包含一个不在 `s` 中的额外字母，并立即返回 `FALSE`。

ttt 是 sss 的异位词等价于「两个字符串中字符出现的种类和次数均相等」。由于字符串只包含 26 个小写字母，因此我们可以维护一个长度为 26 的频次数组 `table`，先遍历记录字符串 `s` 中字符出现的频次，然后遍历字符串 `t`，减去 `table` 中对应的频次，如果出现 `table[i]<0`，则说明 `t` 包含一个不在 `s` 中的额外字符，返回 `false` 即可。

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] table = new int[26];
    for (int i = 0; i < s.length(); i++) {
        table[s.charAt(i) - 'a']++;
    }
    for (int i = 0; i < t.length(); i++) {
        table[t.charAt(i) - 'a']--;
        if (table[t.charAt(i) - 'a'] < 0) {
            return false;
        }
    }
    return true;
}
```
自己的解法:
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int lenS=s.length(),lenT=t.length();
        if(lenS!=lenT){
            return false;
        }
        HashMap<Character,Integer> map=new HashMap<>();
        char[] arr=s.toCharArray();
        for(char c:arr){
            map.put(c,map.getOrDefault(c,0)+1);
        }
        char[] temp=t.toCharArray();
        for(char c:temp){
            if(!map.containsKey(c)){
                return false;
            }else{
                int cnt=map.get(c);
                cnt--;
                if(cnt<0){
                    return false;
                }
                map.put(c,cnt);
            }
        }
        return true;
    }
}
```
或
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        char[] arrS=s.toCharArray();
        char[] arrT=t.toCharArray();
        int[] cnt=new int[26];
        for(char c:arrS){
            cnt[c-'a']++;
        }
        for(char c:arrT){
            cnt[c-'a']--;
        }
        for(int num:cnt){
            if(num!=0){
                return false;
            }
        }
        return true;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n)$。时间复杂度为 $O(n)$ 因为访问计数器表是一个固定的时间操作。
- 空间复杂度：$O(1)$。尽管我们使用了额外的空间，但是空间的复杂性是 $O(1)$，因为无论 `N` 有多大，表的大小都保持不变。
