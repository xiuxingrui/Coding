# [LeetCode567.字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)
## 题目描述
给定两个字符串 `s1` 和 `s2`，写一个函数来判断 `s2` 是否包含 `s1` 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的子串。

- 输入的字符串只包含小写字母
- 两个字符串的长度都在 `[1, 10,000]` 之间
### 示例
```
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").
```
```
输入: s1= "ab" s2 = "eidboaoo"
输出: False
```
## 题解
### 解法一
我们可以为 `s2` 中的第一个窗口创建一次哈希表，而不是为 `s2` 中考虑的每个窗口重新生成哈希表。然后，稍后当我们滑动窗口时，我们知道我们添加了一个前面的字符并将新的后续字符添加到所考虑的新窗口中。因此，我们可以通过仅更新与这两个字符相关联的索引来更新哈希表。同样，对于每个更新的哈希表，我们将哈希表的所有元素进行比较以获得所需的结果。

```java
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()){
            return false;
        }
        int[] s1map = new int[26];
        int[] s2map = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            s1map[s1.charAt(i) - 'a']++;
            s2map[s2.charAt(i) - 'a']++;
        }
        for (int i = 0; i < s2.length() - s1.length(); i++) {
            if (matches(s1map, s2map))
                return true;
            s2map[s2.charAt(i + s1.length()) - 'a']++;
            s2map[s2.charAt(i) - 'a']--;
        }
        return matches(s1map, s2map);
    }
    public boolean matches(int[] s1map, int[] s2map) {
        for (int i = 0; i < 26; i++) {
            if (s1map[i] != s2map[i])
                return false;
        }
        return true;
    }
}
```
或
```java
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length())
            return false;
        char[] ch1=s1.toCharArray();
        char[] ch2=s2.toCharArray();
        int[] s1map = new int[26];
        int[] s2map = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            s1map[ch1[i] - 'a']++;
            s2map[ch2[i] - 'a']++;
        }
        for (int i = 0; i < s2.length() - s1.length(); i++) {
            if (matches(s1map, s2map))
                return true;
            s2map[ch2[i + s1.length()] - 'a']++;
            s2map[ch2[i] - 'a']--;
        }
        return matches(s1map, s2map);
    }
    public boolean matches(int[] s1map, int[] s2map) {
        for (int i = 0; i < 26; i++) {
            if (s1map[i] != s2map[i])
                return false;
        }
        return true;
    }
}
```
- 时间复杂度：$O(l1+26∗(l2−l1))$。其中`l1`是字符串`s1`的长度，`l2`是字符串`s2`的长度。
- 空间复杂度：$O(1)$。常数级空间。
优化：

```java
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length())
            return false;
        int[] s1map = new int[26];
        int[] s2map = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            s1map[s1.charAt(i) - 'a']++;
            s2map[s2.charAt(i) - 'a']++;
        }
        int count = 0;
        for (int i = 0; i < 26; i++)
            if (s1map[i] == s2map[i])
                count++;
        for (int i = 0; i < s2.length() - s1.length(); i++) {
            int r = s2.charAt(i + s1.length()) - 'a', l = s2.charAt(i) - 'a';
            if (count == 26)
                return true;
            s2map[r]++;
            if (s2map[r] == s1map[r])
                count++;
            else if (s2map[r] == s1map[r] + 1)
                count--;
            s2map[l]--;
            if (s2map[l] == s1map[l])
                count++;
            else if (s2map[l] == s1map[l] - 1)
                count--;
        }
        return count == 26;
    }
}
```
或：
```java
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()){
            return false;
        }
        char[] ch1=s1.toCharArray();
        char[] ch2=s2.toCharArray();
        int[] s1map = new int[26];
        int[] s2map = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            s1map[ch1[i]- 'a']++;
            s2map[ch2[i] - 'a']++;
        }
        int count = 0;
        for (int i = 0; i < 26; i++)
            if (s1map[i] == s2map[i])
                count++;
        for (int i = 0; i < s2.length() - s1.length(); i++) {
            int r = ch2[i + s1.length()] - 'a', l = ch2[i] - 'a';
            if (count == 26)
                return true;
            s2map[r]++;
            if (s2map[r] == s1map[r])
                count++;
            else if (s2map[r] == s1map[r] + 1)
                count--;
            s2map[l]--;
            if (s2map[l] == s1map[l])
                count++;
            else if (s2map[l] == s1map[l] - 1)
                count--;
        }
        return count == 26;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(l1+(l2−l1))$。其中`l1`是字符串`s1`的长度，`l2`是字符串`s2`的长度。
- 空间复杂度：$O(1)$。常数级空间。
### 解法二
套模板：

本题移动`left`缩小窗口的时机是窗口大小大于`t.size()`时，因为排列嘛，显然长度应该是一样的。

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        HashMap<Character,Integer> window=new HashMap<>();
        HashMap<Character,Integer> need=new HashMap<>();
        char[] ch1=s1.toCharArray();
        char[] ch2=s2.toCharArray();
        for(char c:ch1){
            need.put(c,need.getOrDefault(c,0)+1);
        }
        int left=0,right=0;
        int valid=0;
        while(right<s2.length()){
            char r=ch2[right];
            right++;
            if(need.containsKey(r)){
                window.put(r,window.getOrDefault(r,0)+1);
                if(window.get(r).equals(need.get(r))){
                    valid++;
                }
            }
            while(right-left>ch1.length){
                char l=ch2[left];
                left++;
                if(window.containsKey(l)){
                    if(window.get(l).equals(need.get(l))){
                        valid--;
                    }
                    window.put(l,window.get(l)-1);
                }
            }
            if(valid==need.size()){
                return true;
            }
        }
        return false;
    }
}
```
自己的写法：
```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        HashMap<Character,Integer> window=new HashMap<>();
        HashMap<Character,Integer> need=new HashMap<>();
        char[] ch1=s1.toCharArray();
        char[] ch2=s2.toCharArray();
        for(char c:ch1){
            need.put(c,need.getOrDefault(c,0)+1);
        }
        int left=0,right=0;
        int valid=0;
        while(right<s2.length()){
            char r=ch2[right];
            right++;
            if(need.containsKey(r)){
                window.put(r,window.getOrDefault(r,0)+1);
                if(window.get(r).equals(need.get(r))){
                    valid++;
                }
            }else{
                window.clear();
                left=right;
                valid=0;
            }
            while(left<right-s1.length()){
                char l=ch2[left];
                if(window.containsKey(l)){
                    if(window.get(l).equals(need.get(l))){
                        valid--;
                    }
                    window.put(l,window.get(l)-1);
                }
                left++;
            }
            if(valid==need.size()){
                return true;
            }
        }
        return false;
    }
}
```
### 解法三
如上所述，只有当两个字符串包含具有相同频率的相同字符时，一个字符串才是另一个字符串的排列。我们可以考虑与 `s1` 长度相同的长字符串 `s2` 中的每个可能的子字符串，并检查出现在两者中的字符出现的频率。如果每个字母的频率完全匹配，则只有 `s1` 的排列可以是 `s2` 的子字符串。

为了实现这种方法，我们不使用排序然后比较元素的相等性，而是使用一个哈希表 `s1map`来存储短字符串 `s1` 中所有字符的出现频率。我们考虑 `s2` 的每个可能的子串，其长度与 `s1` 的长度相同，也可以找到相应的哈希表，即 `s2map`。因此，所考虑的子字符串可以被视为一个长度窗口，如 `s1` 迭代超过 `s2`。如果获得的两个哈希表对于任何这样的窗口是相同的，我们可以得出结论 `s1` 的排列是 `s2` 的子字符串，否则不是。

我们可以使用更简单的数组数据结构来存储频率，而不是仅使用特殊的哈希表数据结构来存储字符出现的频率。给定字符串仅包含小写字母（`'a'`到`'z'`）。因此我们需要采用大小为 26 的数组。其余过程与最后一种方法保持一致。

```java
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length())
            return false;
        int[] s1map = new int[26];
        for (int i = 0; i < s1.length(); i++)
            s1map[s1.charAt(i) - 'a']++;
        for (int i = 0; i <= s2.length() - s1.length(); i++) {
            int[] s2map = new int[26];
            for (int j = 0; j < s1.length(); j++) {
                s2map[s2.charAt(i + j) - 'a']++;
            }
            if (matches(s1map, s2map))
                return true;
        }
        return false;
    }
    public boolean matches(int[] s1map, int[] s2map) {
        for (int i = 0; i < 26; i++) {
            if (s1map[i] != s2map[i])
                return false;
        }
        return true;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(l1+26∗l1∗(l2−l1))$。其中 `l1`是字符串`s1`的长度，`l2`是字符串`s2`的长度。
- 空间复杂度：$O(1)$。使用 `s1map`和 `s2map`，大小为 26。
