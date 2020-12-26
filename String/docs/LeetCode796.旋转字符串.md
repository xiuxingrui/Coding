# [LeetCode796.旋转字符串](https://leetcode-cn.com/problems/rotate-string/)
## 题目描述
给定两个字符串, `A` 和 `B`。

`A` 的旋转操作就是将 `A` 最左边的字符移动到最右边。 例如, 若 `A = 'abcde'`，在移动一次之后结果就是`'bcdea'` 。如果在若干次旋转操作之后，`A` 能变成`B`，那么返回`True`。

- `A` 和 `B` 长度不超过 100。
### 示例
```
示例 1:
输入: A = 'abcde', B = 'cdeab'
输出: true
```
```
示例 2:
输入: A = 'abcde', B = 'abced'
输出: false
```
## 题解
### 解法一
每次将旋转后的`A`和目标串对比：
```java
class Solution {
    public boolean rotateString(String A, String B) {
        if (A.equals("") && B.equals("")) {
            return true;
        }
        int len = A.length();
        for (int i = 1; i < len; i++) {
            String first = A.substring(0, i);
            String last = A.substring(i, len);
            String temp = last + first;
            if (temp.equals(B)) {
                return true;
            }
        }
        return false;
    }
}
```
### 解法二
由于 `A + A` 包含了所有可以通过旋转操作从 `A` 得到的字符串，因此我们只需要判断 `B` 是否为 `A + A` 的子串即可。

```java
class Solution {
    public boolean rotateString(String A, String B) {
        return A.length() == B.length() && (A + A).contains(B);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N^2)$，其中 `N` 是字符串 `A` 的长度。
- 空间复杂度：$O(N)$，即 `A + A` 需要的空间。
### 解法三
使用`KMP`算法判断是否包含子串：
```java
class Solution {
    public boolean rotateString(String A, String B) {
        if(A==null||B==null||A.length()!=B.length()){
            return false;
        }
        if(A.equals("")&&B.equals("")){
            return true;
        }
        String str=A+A;
        char[] s=str.toCharArray();
        char[] t=B.toCharArray();
        int[] nextVal=getNextVal(t);
        int j=-1;
        for(int i=0;i<str.length();i++){
            while(j!=-1&&s[i]!=t[j+1]){
                j=nextVal[j];
            }
            if(s[i]==t[j+1]){
                j++;
            }
            if(j==B.length()-1){
                return true;
            }
        }
        return false;
    }
    public int[] getNextVal(char[] s){
        int len=s.length;
        int[] nextVal=new int[len];
        int j=-1;
        nextVal[0]=j;
        for(int i=1;i<len-1;i++){//nextVal[len-1]没用
            while(j!=-1&&s[i]!=s[j+1]){
                j=nextVal[j];
            }
            if(s[i]==s[j+1]){
                j++;
            }
            if(j==-1||s[i+1]!=s[j+1]){
                nextVal[i]=j;
            }else{
                nextVal[i]=nextVal[j];
            }
        }
        return nextVal;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是字符串 `A` 的长度。
- 空间复杂度：$O(N)$。
