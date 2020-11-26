# [剑指Offer58-I.翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
## 题目描述
输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串`"I am a student. "`，则输出`"student. a am I"`。

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

### 示例
```
输入: "the sky is blue"
输出: "blue is sky the"
```
```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```
```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```
## 题解
### 解法一
以空格为分割符完成字符串分割后，若两单词间有 `x>1` 个空格，则在单词列表 `strs` 中，此两单词间会多出 `x−1` 个 “空单词” （即 `""` ）。解决方法：倒序遍历单词列表，并将单词逐个添加至 `StringBuilder` ，遇到空单词时跳过。

```java
class Solution {
    public String reverseWords(String s) {
        String[] strs=s.trim().split(" ");
        StringBuilder ans=new StringBuilder();
        for(int i=strs.length-1;i>=0;i--){
            if(!strs[i].equals("")){
                ans.append(strs[i]).append(' ');
            }
        }
        return ans.toString().trim();
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(N)$： 总体为线性时间复杂度，`split()` 方法： 为 $O(N)$,`trim()`方法： 最差情况下（当字符串全为空格时），为 $O(N)$；
- 空间复杂度 $O(N)$： 单词列表 `strs` 占用线性大小的额外空间。
### 解法二
- 倒序遍历字符串 `s` ，记录单词左右索引边界 `i` , `j`；
- 每确定一个单词的边界，则将其添加至单词列表 `res` ；
- 最终，将单词列表拼接为字符串，并返回即可。

```java
class Solution {
    public String reverseWords(String s) {
        s=s.trim();// 删除首尾空格
         char[] arr=s.toCharArray();
         StringBuilder ans=new StringBuilder();
         int i=s.length()-1,j=i;
         while(i>=0){
             while(i>=0&&arr[i]!=' '){// 搜索首个空格
                 i--;
             }
             ans.append(s.substring(i+1,j+1)).append(' ');// 添加单词
             while(i>=0&&arr[i]==' '){// 跳过单词间空格
                 i--;
             }
             j=i;// j 指向下个单词的尾字符
         }
         return ans.toString().trim();// 转化为字符串并返回
    }
}
```
#### 复杂度分析：
- 时间复杂度 $O(N)$： 其中 `N` 为字符串 `s` 的长度，线性遍历字符串。
- 空间复杂度 $O(N)$ ： 新建的 `StringBuilder`(Java) 中的字符串总长度 `≤N`，占用 $O(N)$ 大小的额外空间。
