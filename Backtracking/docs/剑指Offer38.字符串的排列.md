# [剑指Offer38.字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)
## 题目描述
输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。
- `1 <= s 的长度 <= 8`
### 示例
```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```
## 题解
### 解法一
判断条件去重：
```java
class Solution {
    public String[] permutation(String s) {
        List<String> ans=new ArrayList<>();
        char[] chs=s.toCharArray();
        boolean[] visited=new boolean[chs.length];
        Arrays.sort(chs);
        backtrack(chs,chs.length,new StringBuilder(),visited,ans);
        return ans.toArray(new String[ans.size()]);
    }
    public void backtrack(char[] chs,int len,StringBuilder sb,boolean[] visited,List<String> ans){
        if(len==0){
            ans.add(sb.toString());
            return;
        }
        for(int i=0;i<chs.length;i++){
            if(visited[i]){
                continue;
            }
            if(i>0&&chs[i]==chs[i-1]&&visited[i-1]==false){
                continue;
            }
            visited[i]=true;
            sb.append(chs[i]);
            backtrack(chs,len-1,sb,visited,ans);
            sb.deleteCharAt(sb.length()-1);
            visited[i]=false;
        }
    }
}
```
### 解法二
`HashSet`去重
```java
class Solution {
    public String[] permutation(String s) {
        List<String> ans=new ArrayList<>();
        char[] chs=s.toCharArray();
        boolean[] visited=new boolean[chs.length];
        backtrack(chs,chs.length,new StringBuilder(),visited,ans);
        return ans.toArray(new String[ans.size()]);
    }
    public void backtrack(char[] chs,int len,StringBuilder sb,boolean[] visited,List<String> ans){
        if(len==0){
            ans.add(sb.toString());
            return;
        }
        HashSet<Character> set=new HashSet<>();
        for(int i=0;i<chs.length;i++){
            if(visited[i]){
                continue;
            }
            if(set.contains(chs[i])){
                continue;
            }
            set.add(chs[i]);
            visited[i]=true;
            sb.append(chs[i]);
            backtrack(chs,len-1,sb,visited,ans);
            sb.delete(sb.length()-1,sb.length());
            visited[i]=false;
        }
    }
}
```
