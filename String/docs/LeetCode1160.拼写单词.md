# [LeetCode1160.拼写单词](https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters/)
## 题目描述
给你一份『词汇表』（字符串数组） `words` 和一张『字母表』（字符串） `chars`。

假如你可以用 `chars` 中的『字母』（字符）拼写出 `words` 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。

注意：每次拼写（指拼写词汇表中的一个单词）时，`chars` 中的每个字母都只能用一次。

返回词汇表 `words` 中你掌握的所有单词的 长度之和。

- `1 <= words.length <= 1000`
- `1 <= words[i].length, chars.length <= 100`
- 所有字符串中都仅包含小写英文字母

## 示例
```
输入：words = ["cat","bt","hat","tree"], chars = "atach"
输出：6
解释： 
可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。
```
```
输入：words = ["hello","world","leetcode"], chars = "welldonehoneyr"
输出：10
解释：
可以形成字符串 "hello" 和 "world"，所以答案是 5 + 5 = 10。
```
## 题解
显然，对于一个单词 `word`，只要其中的每个字母的数量都不大于 `chars` 中对应的字母的数量，那么就可以用 `chars` 中的字母拼写出 `word`。所以我们只需要用一个哈希表存储 `chars` 中每个字母的数量，再用一个哈希表存储 `word` 中每个字母的数量，最后将这两个哈希表的键值对逐一进行比较即可。

```java
class Solution {
    public int countCharacters(String[] words, String chars) {
        int[] c = new int[26];
        for(char cc : chars.toCharArray()) {
            c[cc - 'a'] += 1;
        }
        int res = 0;
        for(String word : words) {
            int[] w = new int[26];
            boolean flag=false;
            for(char ww : word.toCharArray()) {
                w[ww - 'a'] += 1;
            }
            for(int i=0; i<26; i++) {
                if(w[i] > c[i]) {
                    flag=true;
                    break;
                }
            }
            if(!flag){
                res += word.length();
            }
        }
        return res;
    }
}
```
自己的解法：
```java
class Solution {
    public int countCharacters(String[] words, String chars) {
        int[] cnt=new int[26];
        char[] arr=chars.toCharArray();
        for(char c:arr){
            cnt[c-'a']++;
        }
        int ans=0;
        for(int i=0;i<words.length;i++){
            char[] temp=words[i].toCharArray();
            int[] cntTemp=Arrays.copyOf(cnt,cnt.length);
            boolean flag=false;
            for(int j=0;j<temp.length;j++){
                cntTemp[temp[j]-'a']--;
                if(cntTemp[temp[j]-'a']<0){
                    flag=true;
                    break;
                }
            }
            if(!flag){
                ans+=words[i].length();
            }
        }
        return ans;
    }
}
```
### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 为所有字符串的长度和。我们需要遍历每个字符串，包括 `chars` 以及数组 `words` 中的每个单词。
- 空间复杂度：$O(S)$，其中 `S` 为字符集大小，在本题中 `S` 的值为 26（所有字符串仅包含小写字母）。程序运行过程中，最多同时存在两个哈希表，使用的空间均不超过字符集大小 `S`，因此空间复杂度为 $O(S)$。
