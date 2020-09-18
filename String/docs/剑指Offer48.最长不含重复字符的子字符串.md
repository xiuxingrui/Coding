# [剑指Offer48.最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)
## 题目描述
请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

### 示例
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
## 题解
### 解法一
当`window[c]`值大于 1 时，说明窗口中存在重复字符，不符合条件，就该移动`left`缩小窗口。
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character,Integer> map=new HashMap<>();
        char[] chs=s.toCharArray();
        int maxLen=0;
        int left=0,right=0;
        while(right<s.length()){
            char c=chs[right];
            map.put(c,map.getOrDefault(c,0)+1);
            right++;
            while(map.get(c)>1){
                char l=chs[left];
                map.put(l,map.get(l)-1);
                left++;
            }
            maxLen=maxLen>right-left?maxLen:right-left;
        }
        return maxLen;
    }
}
```
自己写法
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s==null||s.length()==0){
            return 0;
        }
        HashMap<Character,Integer> map=new HashMap<>();
        char[] chs=s.toCharArray();
        int maxLen=0;
        int left=0,right=0;
        while(right<s.length()){
            char r=chs[right];
            right++;
            if(map.containsKey(r)==false||map.get(r)==0){
                maxLen=maxLen>right-left?maxLen:right-left;
            }else{
                while(left<right-1){
                    if(chs[left]==r){
                        map.put(r,map.get(r)-1);
                        left++;
                        break;
                    }
                    map.put(chs[left],map.get(chs[left])-1);
                    left++;
                }
            }
            map.put(r,map.getOrDefault(r,0)+1);
        }
        return maxLen;
    }
}
```
### 解法二
- 定义一个 `map` 数据结构存储 `(k, v)`，其中 `key` 值为字符，`value` 值为字符位置 +1，加 1 表示从字符位置后一个才开始不重复
- 我们定义不重复子串的开始位置为 `start`，结束位置为 `end`
- 随着 `end` 不断遍历向后，会遇到与 `[start, end]` 区间内字符相同的情况，此时将字符作为 `key` 值，获取其 `value` 值，并更新 `start`，此时 `[start, end]` 区间内不存在重复字符
- 无论是否更新 `start`，都会更新其`map`数据结构和结果 `ans`。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max = 0;
        int left = 0;
        char[] chs=s.toCharArray();
        for(int i = 0; i < s.length(); i ++){
            if(map.containsKey(chs[i])){
                left = Math.max(left,map.get(chs[i]) + 1);
            }
            map.put(chs[i],i);
            max = Math.max(max,i-left+1);
        }
        return max;
        
    }
}
```
### 解法三
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashSet<Character> set=new HashSet<>();
        char[] charArray=s.toCharArray();
        int len=s.length();
        int ans=0;
        for(int i=0;i<len;i++){
            set.clear();
            int tmepLen=0;
            for(int j=i;j<len;j++){
                if(set.contains(charArray[j])==true){
                    break;
                }
                tmepLen++;
                ans=Math.max(ans,tmepLen);
                set.add(charArray[j]);
            }
        }
        return ans;
    }
}
```