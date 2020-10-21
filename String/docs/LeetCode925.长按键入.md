# [LeetCode925.长按键入](https://leetcode-cn.com/problems/long-pressed-name/)
## 题目描述
你的朋友正在使用键盘输入他的名字 `name`。偶尔，在键入字符 `c` 时，按键可能会被长按，而字符可能被输入 1 次或多次。

你将会检查键盘输入的字符 `typed`。如果它对应的可能是你的朋友的名字（其中一些字符可能被长按），那么就返回 `True`。

- `name.length <= 1000`
- `typed.length <= 1000`
- `name` 和 `typed` 的字符都是小写字母。
### 示例
```
输入：name = "alex", typed = "aaleex"
输出：true
解释：'alex' 中的 'a' 和 'e' 被长按。
```
```
输入：name = "saeed", typed = "ssaaedd"
输出：false
解释：'e' 一定需要被键入两次，但在 typed 的输出中不是这样。
```
```
输入：name = "leelee", typed = "lleeelee"
输出：true
```
```
输入：name = "laiden", typed = "laiden"
输出：true
解释：长按名字中的字符并不是必要的。
```
## 题解
### 解法一
根据题意能够分析得到：字符串 `typed` 的每个字符，有且只有两种「用途」：
- 作为 `name` 的一部分。此时会「匹配」`name` 中的一个字符
- 作为长按键入的一部分。此时它应当与前一个字符相同。

如果 `typed` 中存在一个字符，它两个条件均不满足，则应当直接返回 `false`；否则，当 `typed` 扫描完毕后，我们再检查 `name` 的每个字符是否都被「匹配」了。

实现上，我们使用两个下标 `i`,`j` 追踪 `name` 和 `typed` 的位置。

- 当 `name[i]=typed[j]` 时，说明两个字符串存在一对匹配的字符，此时将 `i`,`j` 都加 1。
- 否则，如果 `typed[j]=typed[j−1]`，说明存在一次长按键入，此时只将 `j` 加 1。

最后，如果 `i=name.length`，说明 `name` 的每个字符都被「匹配」了。

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        char[] s=name.toCharArray();
        char[] t=typed.toCharArray();
        int i = 0, j = 0;
        while (j < typed.length()) {
            if (i < name.length() && s[i]==t[j]) {
                i++;
                j++;
            } else if (j > 0 && t[j]== t[j-1]) {
                j++;
            } else {
                return false;
            }
        }
        return i == name.length();
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N+M)$，其中 `M`,`N` 分别为两个字符串的长度。
- 空间复杂度：$O(1)$。

### 解法二
写法一
```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {

        if(typed.length()<name.length()){
            return false;
        }
        char[] s=name.toCharArray();
        char[] t=typed.toCharArray();
        int idx1=0,idx2=0;
        while(idx1<name.length()){
            char c=s[idx1];
            int i=idx1;
            while(i<name.length()&&s[i]==c){
                i++;
            }
            if(idx2==typed.length()){
                return false;
            }
            int j=idx2;
            while(j<typed.length()&&t[j]==c){
                j++;
            }
            if(j-idx2<i-idx1){
                return false;
            }
            idx2=j;
            idx1=i;
        }
        if(idx2==typed.length()){
            return true;
        }else{
            return false;
        }
    }
}
```
写法二:
```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {

        if(typed.length()<name.length()){
            return false;
        }
        char[] s=name.toCharArray();
        char[] t=typed.toCharArray();
        int idx1=0,idx2=0;
        while(idx1<name.length()){
            char c=s[idx1];
            int cnt1=1;
            while(idx1<name.length()-1&&s[idx1]==s[idx1+1]){
                idx1++;
                cnt1++;
            }
            idx1++;
            if(idx2==typed.length()){
                return false;
            }
            if(t[idx2]!=c){
                return false;
            }else{
                int cnt2=1;
                while(idx2<typed.length()-1&&t[idx2+1]==t[idx2]){
                    idx2++;
                    cnt2++;
                }
                idx2++;
                if(cnt2<cnt1){
                    return false;
                }
            }
        }
        if(idx2==typed.length()){
            return true;
        }else{
            return false;
        }
    }
}
```