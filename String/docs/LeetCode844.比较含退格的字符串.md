# [LeetCode844.比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)
## 题目描述
给定 `S` 和 `T` 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 `#` 代表退格字符。

注意：如果对空文本输入退格字符，文本继续为空。

### 示例
```
输入：S = "ab#c", T = "ad#c"
输出：true
解释：S 和 T 都会变成 “ac”。
```
```
输入：S = "ab##", T = "c#d#"
输出：true
解释：S 和 T 都会变成 “”。
```
```
输入：S = "a##c", T = "#a#c"
输出：true
解释：S 和 T 都会变成 “c”。
```
```
输入：S = "a#c", T = "b"
输出：false
解释：S 会变成 “c”，但 T 仍然是 “b”。
```
## 题解
### 解法一
一个字符是否会被删掉，只取决于该字符后面的退格符，而与该字符前面的退格符无关。因此当我们逆序地遍历字符串，就可以立即确定当前字符是否会被删掉。

具体地，我们定义 `skip` 表示当前待删除的字符的数量。每次我们遍历到一个字符：

若该字符为退格符，则我们需要多删除一个普通字符，我们让 `skip` 加 1；

若该字符为普通字符：

若 `skip` 为 0，则说明当前字符不需要删去；

若 `skip` 不为 0，则说明当前字符需要删去，我们让 `skip` 减 1。

这样，我们定义两个指针，分别指向两字符串的末尾。每次我们让两指针逆序地遍历两字符串，直到两字符串能够各自确定一个字符，然后将这两个字符进行比较。重复这一过程直到找到的两个字符不相等，或遍历完字符串为止。

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                if (S.charAt(i) == '#') {
                    skipS++;
                    i--;
                } else if (skipS > 0) {
                    skipS--;
                    i--;
                } else {
                    break;
                }
            }
            while (j >= 0) {
                if (T.charAt(j) == '#') {
                    skipT++;
                    j--;
                } else if (skipT > 0) {
                    skipT--;
                    j--;
                } else {
                    break;
                }
            }
            if (i >= 0 && j >= 0) {
                if (S.charAt(i) != T.charAt(j)) {
                    return false;
                }
            } else {
                if (i >= 0 || j >= 0) {
                    return false;
                }
            }
            i--;
            j--;
        }
        return true;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N+M)$，其中 `N` 和 `M` 分别为字符串 `S` 和 `T` 的长度。我们需要遍历两字符串各一次。
- 空间复杂度：$O(1)$。对于每个字符串，我们只需要定义一个指针和一个计数器即可。

### 解法二
最容易想到的方法是将给定的字符串中的退格符和应当被删除的字符都去除，还原给定字符串的一般形式。然后直接比较两字符串是否相等即可。

具体地，我们用栈处理遍历过程，每次我们遍历到一个字符：

如果它是退格符，那么我们将栈顶弹出；

如果它是普通字符，那么我们将其压入栈中。
```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        return build(S).equals(build(T));
    }
    public String build(String str) {
        StringBuilder ret = new StringBuilder();
        int length = str.length();
        for (int i = 0; i < length; ++i) {
            char ch = str.charAt(i);
            if (ch != '#') {
                ret.append(ch);
            } else {
                if (ret.length() > 0) {
                    ret.deleteCharAt(ret.length() - 1);
                }
            }
        }
        return ret.toString();
    }
}
```
自己的解法
```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        StringBuilder sb1=new StringBuilder();
        StringBuilder sb2=new StringBuilder();
        char[] s=S.toCharArray();
        char[] t=T.toCharArray();
        int idxS=-1,idxT=-1;
        for(int i=0;i<S.length();i++){
            if(s[i]!='#'){
                sb1.append(s[i]);
                idxS++;
            }else{
                if(idxS!=-1){
                    sb1.deleteCharAt(idxS--);
                }
            }
        }
        for(int i=0;i<T.length();i++){
            if(t[i]!='#'){
                sb2.append(t[i]);
                idxT++;
            }else{
                if(idxT!=-1){
                    sb2.deleteCharAt(idxT--);
                }
            }
        }
        return (sb1.toString()).equals(sb2.toString());
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N+M)$，其中 `N` 和 `M` 分别为字符串 `S` 和 `T` 的长度。我们需要遍历两字符串各一次。
- 空间复杂度：$O(N+M)$，其中 `N` 和 `M` 分别为字符串 `S` 和 `T` 的长度。主要为还原出的字符串的开销。
