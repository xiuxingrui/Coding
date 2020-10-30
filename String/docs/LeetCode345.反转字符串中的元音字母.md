# [LeetCode345.反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)
## 题目描述
编写一个函数，以字符串作为输入，反转该字符串中的元音字母。

元音字母不包含字母 `"y"` 。
### 示例
```
输入："hello"
输出："holle"
```
```
输入："leetcode"
输出："leotcede"
```
## 题解
### 解法一
本题为基础双指针法交换前后元音元素；

- 一般遇见字符串问题，能转成字符数组就尽量转(方便)；
- 转换成数组后，分别定义前后两个索引指针用 `while` 依次遍历数组；
- 定义 `isVowel()` 方法将非元音元素返回给判断处，然后移动指针直到符合元音的位置，然后 `tmp` 进行交换即可；

最后扫描完数组后，一定要在返回的时候再转成字符串 `String` 输出。

```java
class Solution {
    public String reverseVowels(String s) {
        // 先将字符串转成字符数组（方便操作）
        // 以上是只针对 Java 语言来说的 因为 chatAt(i) 每次都要检查是否越界 有性能消耗
        char[] arr = s.toCharArray();
        int n = arr.length;
        int l = 0;
        int r = n - 1;
        while (l < r) {
            // 从左判断如果当前元素不是元音
            while (l < n && !isVowel(arr[l]) ) {
                l++;
            }
            // 从右判断如果当前元素不是元音
            while (r >= 0 && !isVowel(arr[r]) ) {
                r--;
            }
            // 如果没有元音
            if (l >= r) {
                break;
            }
            // 交换前后的元音
            swap(arr, l, r);
            // 这里要分开写，不要写进数组里面去
            l++;
            r--;
        }
        // 最后返回的时候要转换成字符串输出
        return new String(arr);
    }

    private void swap(char[] arr, int a, int b) {
        char tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }

    // 判断是不是元音
    private boolean isVowel(char ch) {
        // 这里要直接用 return 语句返回，不要返回 true 或者 false
         return ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u'
                ||ch=='A'|| ch == 'E' || ch == 'I' || ch == 'O' || ch == 'U';
    }
}
```
```java
class Solution {
    public String reverseVowels(String s) {
        if (s == null || s.length() == 0) {
            return s;
        }
        String vowels = "aeiouAEIOU";
        
        // 将字符串转化成char类型数组
        char[] chars = s.toCharArray();
        int start = 0;
        int end = s.length() - 1;
        while (start < end) {
            // 双指针相向而行找元音字符
            while (start < end && !vowels.contains(chars[start] + "")) {
                start++;
            }
            while (start < end && !vowels.contains(chars[end] + "")) {
                end--;
            }
            swap(chars, start, end);
            start++;
            end--;
        }
        return new String(chars);
    }
    
    private void swap(char[] chars, int start, int end) {
        char temp = chars[start];
        chars[start] = chars[end];
        chars[end] = temp;
    }
}
```
```java
class Solution {
    public String reverseVowels(String s) {
        char[] arr=s.toCharArray();
        HashSet<Character> set=new HashSet<>(){{
                add('a');
                add('e');
                add('i');
                add('o');
                add('u');
                add('A');
                add('E');
                add('I');
                add('O');
                add('U');
            }
        };
        StringBuilder sb=new StringBuilder();
        int len=s.length();
        int j=len-1;
        for(int i=0;i<len;i++){
            if(set.contains(arr[i])){
                while(j>=0&&!set.contains(arr[j])){
                    j--;
                }
                sb.append(arr[j]);
                j--;
            }else{
                sb.append(arr[i]);
            }
        }
        return sb.toString();
    }
}
```
### 解法二
```java
class Solution {
    public String reverseVowels(String s) {
        char[] arr=s.toCharArray();
        HashSet<Character> set=new HashSet<>(){{
                add('a');
                add('e');
                add('i');
                add('o');
                add('u');
                add('A');
                add('E');
                add('I');
                add('O');
                add('U');
            }
        };
        Deque<Character> stack=new LinkedList<>();
        StringBuilder sb=new StringBuilder();
        int len=s.length();
        boolean[] flag=new boolean[len];
        for(int i=0;i<len;i++){
            if(set.contains(arr[i])){
                flag[i]=true;
                stack.push(arr[i]);
            }
        }
        for(int i=0;i<len;i++){
            if(flag[i]){
                sb.append(stack.pop());
            }else{
                sb.append(arr[i]);
            }
        }
        return sb.toString();
    }
}
```

