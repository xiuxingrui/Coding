# [LeetCode131.分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)
## 题目描述
给定一个字符串 `s`，将 `s` 分割成一些子串，使每个子串都是回文串。

返回 `s` 所有可能的分割方案。

### 示例
```
输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```
## 题解
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201003231644.png)

1. 每一个结点表示剩余没有扫描到的字符串，产生分支是截取了剩余字符串的前缀；

2. 产生前缀字符串的时候，判断前缀字符串是否是回文。
- 如果前缀字符串是回文，则可以产生分支和结点；
- 如果前缀字符串不是回文，则不产生分支和结点，这一步是剪枝操作。
3. 在叶子结点是空字符串的时候结算，此时从根结点到叶子结点的路径，就是结果集里的一个结果，使用深度优先遍历，记录下所有可能的结果。
```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> ans=new ArrayList<>();
        int len=s.length();
        backtrack(s,len,0,ans,new ArrayList<String>());
        return ans;
    }   
    public void backtrack(String s,int len,int start,List<List<String>> ans,List<String> path){
        if(start==len){
            ans.add(new ArrayList<>(path));
            return;
        }
        for(int i=start;i<len;i++){
            if(!isPalidrome(s,start,i)){
                continue;
            }
            path.add(s.substring(start,i+1));
            backtrack(s,len,i+1,ans,path);
            path.remove(path.size()-1);
        }
    }
    public boolean isPalidrome(String s,int left,int right){
        while(left<right){
            if(s.charAt(left)!=s.charAt(right)){
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```
用动态规划改进：
```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> ans=new ArrayList<>();
        int len=s.length();
        boolean[][] dp=new boolean[len][len];
        for(int i=0;i<len;i++){
            dp[i][i]=true;
        }
        for(int l=2;l<=len;l++){
            for(int i=0;i+l-1<len;i++){
                int j=i+l-1;
                if(l==2){
                    dp[i][j]=s.charAt(i)==s.charAt(j);
                }else{
                    if(s.charAt(i)==s.charAt(j)){
                        dp[i][j]=dp[i+1][j-1];
                    }else{
                        dp[i][j]=false;
                    }
                }
            }
        }
        backtrack(s,len,0,ans,new ArrayList<String>(),dp);
        return ans;
    }   
    public void backtrack(String s,int len,int start,List<List<String>> ans,List<String> path,boolean[][] dp){
        if(start==len){
            ans.add(new ArrayList<>(path));
            return;
        }
        for(int i=start;i<len;i++){
            if(!dp[start][i]){
                continue;
            }
            path.add(s.substring(start,i+1));
            backtrack(s,len,i+1,ans,path,dp);
            path.remove(path.size()-1);
        }
    }
}
```