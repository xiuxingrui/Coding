# [LeetCode67.二进制求和](https://leetcode-cn.com/problems/add-binary/)
## 题目描述
给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 非空 字符串且只包含数字 1 和 0。

- 每个字符串仅由字符 '0' 或 '1' 组成。
- `1 <= a.length, b.length <= 10^4`
- 字符串如果不是 "0" ，就都不含前导零。
### 示例
```
输入: a = "11", b = "1"
输出: "100"
```
```
输入: a = "1010", b = "1011"
输出: "10101"
```
## 题解
参见`LeetCode415.字符串相加`。
```java
class Solution {
    public String addBinary(String a, String b) {
        char[] arrA=a.toCharArray();
        char[] arrB=b.toCharArray();
        StringBuilder sb=new StringBuilder();
        int i=a.length()-1,j=b.length()-1,flag=0;
        while(i>=0||j>=0||flag!=0){
            int numA=i>=0?arrA[i--]-'0':0;
            int numB=j>=0?arrB[j--]-'0':0;
            int sum=numA+numB+flag;
            // stack.push(sum%2);
            sb.append(sum%2);
            flag=sum/2;
        }
        sb.reverse();
        return sb.toString();
    }
}
```
```java
class Solution {
    public String addBinary(String a, String b) {
        char[] arrA=a.toCharArray();
        char[] arrB=b.toCharArray();
        StringBuilder sb=new StringBuilder();
        Deque<Integer> stack=new LinkedList<>();
        int i=a.length()-1,j=b.length()-1,flag=0;
        while(i>=0||j>=0||flag!=0){
            int numA=i>=0?arrA[i--]-'0':0;
            int numB=j>=0?arrB[j--]-'0':0;
            int sum=numA+numB+flag;
            stack.push(sum%2);
            sb.append(sum%2);
            flag=sum/2;
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

