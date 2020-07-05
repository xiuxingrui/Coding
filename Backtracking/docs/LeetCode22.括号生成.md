# [LeetCode22.括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
## 题目描述
数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

### 示例
```
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```
## 题解
### 解法一
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        backtrack(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, StringBuilder cur, int open, int close, int max){
        if (cur.length() == max * 2) {
            ans.add(cur.toString());
            return;
        }

        if (open < max) {
            cur.append('(');
            backtrack(ans, cur, open+1, close, max);
            cur.deleteCharAt(cur.length() - 1);
        }
        if (close < open) {
            cur.append(')');
            backtrack(ans, cur, open, close+1, max);
            cur.deleteCharAt(cur.length() - 1);
        }
    }
}
```
#### 复杂度分析
- 时间复杂度:$O\left(\frac{4^{n}}{\sqrt{n}}\right)$,在回溯过程中，每个答案需要 $O(n)$ 的时间复制到答案数组中。
- 空间复杂度：$O(n)$，除了答案数组之外，我们所需要的空间取决于递归栈的深度，每一层递归函数需要 $O(1)$ 的空间，最多递归 `2n` 层，因此空间复杂度为 $O(n)$。
### 解法二
回溯+剪枝
```java
class Solution {
    private List<String> result;
    public List<String> generateParenthesis(int n) {
        result=new ArrayList<String>();
        _generate(0,0,n,"");
        return result;
    }
    private void _generate(int left,int right,int n,String s){
        if(left==n&&right==n){
            result.add(s);
            return;
        }
        if(left<n){
            _generate(left+1,right,n,s+"(");
        }
        if(right<left){
            _generate(left,right+1,n,s+")");
        }
    }
}
```
### 解法三
回溯:
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> combinations=new ArrayList<>();
        generateAll(new char[2*n],0,combinations);
        return combinations;
    }
    public void generateAll(char[] current,int pos,List<String> result){
        if(current.length==pos){
            if(valid(current)){
                result.add(new String(current));
            }
            return;
        }
        current[pos]='(';
        generateAll(current,pos+1,result);
        current[pos]=')';
        generateAll(current,pos+1,result);
    }
    public boolean valid(char[] current){
        int balance=0;
        for(char c:current){
            if(c=='(') ++balance;
            else --balance;
            if(balance<0){
                return false;
            }
        }
        return  (balance==0);
    }
}
```
### 解法四
动态规划

