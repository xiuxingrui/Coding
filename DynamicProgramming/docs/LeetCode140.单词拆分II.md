# [LeetCode140.单词拆分II](https://leetcode-cn.com/problems/word-break-ii/)
## 题目描述
给定一个非空字符串 `s` 和一个包含非空单词列表的字典 `wordDict`，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

- 分隔时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

### 示例
```
输入:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
输出:
[
  "cats and dog",
  "cat sand dog"
]
```
```
输入:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
输出:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
解释: 注意你可以重复使用字典中的单词。
```
```
输入:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
输出:
[]
```
## 题解
这道题是「139. 单词拆分」的进阶，第 139 题要求判断是否可以拆分，这道题要求返回所有可能的拆分结果。

第 139 题可以使用动态规划的方法判断是否可以拆分，因此这道题也可以使用动态规划的思想。但是这道题如果使用自底向上的动态规划的方法进行拆分，则无法事先判断拆分的可行性，在不能拆分的情况下会超时。

例如以下例子，由于字符串 `s` 中包含字母 `b`，而单词列表 `wordDict` 中的所有单词都由字母 `a` 组成，不包含字母 `b`，因此不能拆分，但是自底向上的动态规划仍然会在每个下标都进行大量的匹配，导致超时。

```
s = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
wordDict = ["a","aa","aaa","aaaa","aaaaa","aaaaaa","aaaaaaa","aaaaaaaa","aaaaaaaaa","aaaaaaaaaa"]
```
为了避免动态规划的方法超时，需要首先使用第 139 题的代码进行判断，在可以拆分的情况下再使用动态规划的方法进行拆分。

```java
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        HashSet<String> set=new HashSet<>(wordDict);
        List<String> ans=new ArrayList<>();
        int len=s.length();
        boolean[] dp=new boolean[len+1];
        dp[0]=true;
        for(int i=1;i<=len;i++){
            for(int j=1;j<=i;j++){
                if(set.contains(s.substring(j-1,i))){
                    dp[i]=dp[j-1];
                    if(dp[i]){
                        break;
                    }
                }
            }
        }
        if(!dp[len]){//不判断的话会超时
            return ans;
        }
        backtracking(s,1,dp,set,new ArrayList<String>(),ans);
        return ans;
    }
    public void backtracking(String s,int start,boolean[] dp,HashSet<String> set,List<String> path,List<String> ans){
        if(start==s.length()+1){
            String res=String.join(" ",path);

            ans.add(res);
            return;
        }
        for(int i=start;i<=s.length();i++){
            String temp=s.substring(start-1,i);
            if(dp[start-1]&&set.contains(temp)){
                path.add(temp);
                backtracking(s,i+1,dp,set,path,ans);
                path.remove(path.size()-1);
            }
        }
    }
}
```