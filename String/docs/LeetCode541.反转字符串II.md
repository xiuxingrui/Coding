# [LeetCode541.反转字符串II](https://leetcode-cn.com/problems/reverse-string-ii/)
## 题目描述
给定一个字符串 `s` 和一个整数 `k`，你需要对从字符串开头算起的每隔 `2k` 个字符的前 `k` 个字符进行反转。

- 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
- 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

- 该字符串只包含小写英文字母。
- 给定字符串的长度和 `k` 在 `[1, 10000]` 范围内。

### 示例
```
输入: s = "abcdefg", k = 2
输出: "bacdfeg"
```
## 题解
### 解法一
我们直接翻转每个 `2k` 字符块。

每个块开始于 `2k` 的倍数，也就是 `0, 2k, 4k, 6k, ...`。需要注意的一件是：如果没有足够的字符，我们并不需要翻转这个块。

为了翻转从 `i` 到 `j` 的字符块，我们可以交换位于 `i++` 和 `j--` 的字符。

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] a = s.toCharArray();
        for (int start = 0; start < a.length; start += 2 * k) {
            int i = start, j = Math.min(start + k - 1, a.length - 1);
            while (i < j) {
                char tmp = a[i];
                a[i++] = a[j];
                a[j--] = tmp;
            }
        }
        return new String(a);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 是 `s` 的大小。我们建立一个辅助数组，用来翻转 `s` 的一半字符。
- 空间复杂度：$O(N)$，`a` 的大小。

### 解法二
自己的写法
```java
class Solution {
    public String reverseStr(String s, int k) {
        StringBuilder sb=new StringBuilder();
        char[] chs=s.toCharArray();
        int len=chs.length;
        int i=0,j=2*k-1;
        while(j<len){
            reverse(chs,i,i+k-1);
            i=j+1;
            j=i+2*k-1;
        }
        if(i+k-1<len){
            reverse(chs,i,i+k-1);
        }else{
            reverse(chs,i,len-1);
        }
        return new String(chs);
    }
    public void reverse(char[] chs,int left,int right){
        while(left<right){
            char temp=chs[left];
            chs[left]=chs[right];
            chs[right]=temp;
            left++;
            right--;
        }
    }
}
```
