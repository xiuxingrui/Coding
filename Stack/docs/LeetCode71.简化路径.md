# [LeetCode71.简化路径](https://leetcode-cn.com/problems/simplify-path/)
## 题目描述
以 `Unix` 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 `Unix` 风格的文件系统中，一个点（`.`）表示当前目录本身；此外，两个点 （`..`） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：[Linux/Unix中的绝对路径vs相对路径](https://blog.csdn.net/u011327334/article/details/50355600)

请注意，返回的规范路径必须始终以斜杠 `/` 开头，并且两个目录名之间必须只有一个斜杠 `/`。最后一个目录名（如果存在）不能以 `/` 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

### 示例
```
输入："/home/"
输出："/home"
解释：注意，最后一个目录名后面没有斜杠。
```
```
输入："/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。
```
```
输入："/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。
```
```
输入："/a/./b/../../c/"
输出："/c"
```
```
输入："/a/../../b/../c//.//"
输出："/c"
```
```
输入："/a//b////c/d//././/.."
输出："/a/b/c"
```
## 题解
注意：两个以上的`.`可以认为是某个文件名或某个文件的开头。

- 首先定义栈用来存储路径信息，定义字符数组 `str` 来分隔字符串
- 依次遍历字符数组内容，这里使用增强型 `for` 循环，如果是 `“..”` 还要再判断是否为空才能弹出栈
- 如果不为空也不为 `“.”` 这说明当前元素是路径信息，入栈即可
- 最后遍历完之后，先判断栈中是否有元素，没有则返回 `“/”`
- 如果有元素，则使用 `StringBuilder` 来存放可变字符串，最后返回 `ans` 即可。

```java
class Solution {
    public String simplifyPath(String path) {
        Deque<String> deque=new LinkedList<>();
        for(String s:path.split("/")){
            if(s.equals("..")){
                if(!deque.isEmpty()){
                    deque.pollLast();
                }
            }else if(!s.isEmpty()&&!s.equals(".")){
                deque.offerLast(s);
            }
        }
        if(deque.isEmpty()){
            return "/";
        }
        StringBuilder ans=new StringBuilder();
        while(!deque.isEmpty()){
            ans.append('/').append(deque.pollFirst());
        }
        return ans.toString();
    }
}
```