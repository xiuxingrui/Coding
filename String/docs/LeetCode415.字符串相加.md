# [LeetCode415.字符串相加](https://leetcode-cn.com/problems/add-strings/)
## 题目描述
给定两个字符串形式的非负整数 `num1` 和`num2` ，计算它们的和。

提示：

- `num1` 和`num2` 的长度都小于 5100
- `num1` 和`num2` 都只包含数字 0-9
- `num1` 和`num2` 都不包含任何前导零
- 你不能使用任何內建 `BigInteger` 库， 也不能直接将输入的字符串转换为整数形式

## 题解
具体实现也不复杂，我们定义两个指针 `i` 和 `j` 分别指向 `num1` 和 `num2` 的末尾，即最低位，同时定义一个变量`add`维护当前是否有进位，然后从末尾到开头逐位相加即可。你可能会想两个数字位数不同怎么处理，这里我们统一在指针当前下标处于负数的时候返回 0，等价于对位数较短的数字进行了补零操作，这样就可以除去两个数字位数不同情况的处理，具体可以看下面的代码。


```java
class Solution {
    public String addStrings(String num1, String num2) {
        char[] arrA=num1.toCharArray();
        char[] arrB=num2.toCharArray();
        StringBuilder sb=new StringBuilder();
        int i=num1.length()-1,j=num2.length()-1,flag=0;
        while(i>=0||j>=0||flag!=0){
            int numA=i>=0?arrA[i--]-'0':0;
            int numB=j>=0?arrB[j--]-'0':0;
            int sum=numA+numB+flag;
            sb.append(sum%10);
            flag=sum/10;
        }
        sb.reverse();
        return sb.toString();
    }
}
```
```java
class Solution {
    public String addStrings(String num1, String num2) {
        char[] arrA=num1.toCharArray();
        char[] arrB=num2.toCharArray();
        StringBuilder sb=new StringBuilder();
        Deque<Integer> stack=new LinkedList<>();
        int i=num1.length()-1,j=num2.length()-1,flag=0;
        while(i>=0||j>=0||flag!=0){
            int numA=i>=0?arrA[i--]-'0':0;
            int numB=j>=0?arrB[j--]-'0':0;
            int sum=numA+numB+flag;
            stack.push(sum%2);
            sb.append(sum%10);
            flag=sum/10;
        }
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        return sb.toString();
    }
}
```
### 复杂度分析
- 时间复杂度：$O(max(len1,len2))$，其中 `len1=num1.length`，`len2=num2.length`。竖式加法的次数取决于较大数的位数。
- 空间复杂度：$O(1)$。除答案外我们只需要常数空间存放若干变量。在 `Java` 解法中使用到了 `StringBuilder`，故 `Java` 解法的空间复杂度为 $O(n)$。
