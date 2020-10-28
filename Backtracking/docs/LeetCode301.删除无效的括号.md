# [LeetCode301.删除无效的括号](https://leetcode-cn.com/problems/remove-invalid-parentheses/)
## 题目描述
删除最小数量的无效括号，使得输入的字符串有效，返回所有可能的结果。

说明: 输入可能包含了除 `(` 和 `)` 以外的字符。

### 示例
```
输入: "()())()"
输出: ["()()()", "(())()"]
```
```
输入: "(a)())()"
输出: ["(a)()()", "(a())()"]
```
```
输入: ")("
输出: [""]
```
## 题解
```
class Solution {
    int max=Integer.MIN_VALUE;
    public List<String> removeInvalidParentheses(String s) {
        List<String> ans=new ArrayList<>();
        backtracking(s.toCharArray(),0,0,0,new StringBuilder(),ans,new HashSet<String>());
        if(ans.size()==0){
            ans.add("");
            return ans;
        }
        return ans;
    }
    public void backtracking(char[] arr,int i,int l,int r,StringBuilder sb,List<String> ans,HashSet<String> set){
        if(i==arr.length){
            if(l==r){
                if(sb.length()==max){
                    String temp=sb.toString();
                    if(!set.contains(temp)){
                        ans.add(temp);
                        set.add(temp);
                    }
                }
                if(sb.length()>max){
                    set.clear();
                    ans.clear();
                    max=sb.length();
                    String temp=sb.toString();
                    ans.add(temp);
                    set.add(temp);
                }
            }
            return;
        }
        if(arr[i]==')'){
            if(l>r){
                sb.append(')');
                backtracking(arr,i+1,l,r+1,sb,ans,set);
                sb.deleteCharAt(sb.length()-1);
                backtracking(arr,i+1,l,r,sb,ans,set);
            }else{
                backtracking(arr,i+1,l,r,sb,ans,set);
            }
        }else if(arr[i]=='('){
            sb.append('(');
            backtracking(arr,i+1,l+1,r,sb,ans,set);
            sb.deleteCharAt(sb.length()-1);
            backtracking(arr,i+1,l,r,sb,ans,set);
        }else{
            sb.append(arr[i]);
            backtracking(arr,i+1,l,r,sb,ans,set);
            sb.deleteCharAt(sb.length()-1);
        }
    }
}
```
或
```java
class Solution {
    int max=Integer.MIN_VALUE;
    public List<String> removeInvalidParentheses(String s) {
        HashSet<String> set=new HashSet<>();
        backtracking(s.toCharArray(),0,0,0,new StringBuilder(),set);
        if(set.size()==0){
            set.add("");
        }
        return new ArrayList<>(set);
    }
    public void backtracking(char[] arr,int i,int l,int r,StringBuilder sb,HashSet<String> set){
        if(i==arr.length){
            if(l==r){
                if(sb.length()==max){
                    set.add(sb.toString());
                }
                if(sb.length()>max){
                    set.clear();
                    max=sb.length();
                    set.add(sb.toString());
                }
            }
            return;
        }
        if(arr[i]==')'){
            if(l>r){
                sb.append(')');
                backtracking(arr,i+1,l,r+1,sb,set);
                sb.deleteCharAt(sb.length()-1);
                backtracking(arr,i+1,l,r,sb,set);
            }else{
                backtracking(arr,i+1,l,r,sb,set);
            }
        }else if(arr[i]=='('){
            sb.append('(');
            backtracking(arr,i+1,l+1,r,sb,set);
            sb.deleteCharAt(sb.length()-1);
            backtracking(arr,i+1,l,r,sb,set);
        }else{
            sb.append(arr[i]);
            backtracking(arr,i+1,l,r,sb,set);
            sb.deleteCharAt(sb.length()-1);
        }
    }
}
```

