# [LeetCode127.单词接龙](https://leetcode-cn.com/problems/word-ladder/)
## 题目描述
给定两个单词（`beginWord` 和 `endWord`）和一个字典，找到从 `beginWord` 到 `endWord` 的最短转换序列的长度。转换需遵循如下规则：

- 每次转换只能改变一个字母。
- 转换过程中的中间单词必须是字典中的单词。

说明:

- 如果不存在这样的转换序列，返回 0。
- 所有单词具有相同的长度。
- 所有单词只由小写字母组成。
- 字典中不存在重复的单词。
- 你可以假设 `beginWord` 和 `endWord` 是非空的，且二者不相同。

### 示例
```
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
```
```
输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。
```
## 题解
### 解法一
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if(!wordList.contains(endWord)){
            return 0;
        }
        Queue<String> queue=new LinkedList<>();
        boolean[] visited=new boolean[wordList.size()];
        int idx=wordList.indexOf(beginWord);
        if(idx!=-1){
            visited[idx]=true;
        }
        int cnt=0;
        queue.offer(beginWord);
        while(!queue.isEmpty()){
            int size=queue.size();
            cnt++;
            for(int i=0;i<size;i++){
                String s=queue.poll();
                if(s.equals(endWord)){
                    return cnt;
                }
                for(int j=0;j<wordList.size();j++){
                    String temp=wordList.get(j);
                    if(visited[j]||!check(s,temp)){
                        continue;
                    }
                    visited[j]=true;
                    queue.offer(temp);
                }
            }
        }
        return 0;
    }
    public boolean check(String s,String p){
        char[] arrS=s.toCharArray();
        char[] arrP=p.toCharArray();
        int cnt=0;
        for(int i=0;i<s.length();i++){
            if(arrS[i]!=arrP[i]){
                cnt++;
                if(cnt>1){
                    return false;
                }
            }
        }
        return cnt==1;
    }
}
```
### 解法二
`DFS`:超时
```java
class Solution {
    int ans=Integer.MAX_VALUE;
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if(!wordList.contains(endWord)){
            return 0;
        }
        boolean[] visited=new boolean[wordList.size()];
        backtracking(beginWord,beginWord,endWord,wordList,1,visited);
        return ans==Integer.MAX_VALUE?0:ans;
    }
    public void backtracking(String s,String beginWord,String endWord,List<String> wordList,int cnt,boolean[] visited){
        if(s.equals(endWord)){
            ans=Math.min(cnt,ans);
            return;
        }
        for(int i=0;i<wordList.size();i++){
            if(visited[i]||!check(s,wordList.get(i))||wordList.get(i).equals(beginWord)){
                continue;
            }
            visited[i]=true;
            backtracking(wordList.get(i),beginWord,endWord,wordList,cnt+1,visited);
            visited[i]=false;
        }
    }
    public boolean check(String s,String p){
        char[] arrS=s.toCharArray();
        char[] arrP=p.toCharArray();
        int len=s.length();
        int cnt=0;
        for(int i=0;i<len;i++){
            if(arrP[i]!=arrS[i]){
                cnt++;
            }
        }
        return cnt==1;
    }
}
```