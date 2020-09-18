# [LeetCode438.找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)
## 题目描述
给定一个字符串 `s` 和一个非空字符串 `p`，找到 `s` 中所有是 `p` 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 `s` 和 `p` 的长度都不超过 20100。

- 字母异位词指字母相同，但排列不同的字符串。
- 不考虑答案输出的顺序。

### 示例
```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```
```
输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。
```
## 题解
```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans=new ArrayList<>();
        HashMap<Character,Integer> window=new HashMap<>();
        HashMap<Character,Integer> need=new HashMap<>();
        char[] chs=s.toCharArray();
        char[] chp=p.toCharArray();
        for(int i=0;i<p.length();i++){
            need.put(chp[i],need.getOrDefault(chp[i],0)+1);
        }
        int left=0,right=0;
        int valid=0;
        while(right<s.length()){
            char r=chs[right];
            right++;
            if(need.containsKey(r)){
                window.put(r,window.getOrDefault(r,0)+1);
                if(window.get(r).equals(need.get(r))){
                    valid++;
                }
            }
            while(left<right-p.length()){
                char l=chs[left];
                left++;
                if(need.containsKey(l)){
                    window.put(l,window.get(l)-1);
                    if(window.get(l)<need.get(l)){
                        valid--;
                    }
                }
            }
            if(valid==need.size()){
                ans.add(left);
            }
        }
        return ans;
    }
}
```