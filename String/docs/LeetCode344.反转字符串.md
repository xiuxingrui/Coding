# [LeetCode344.反转字符串](https://leetcode-cn.com/problems/reverse-string/)
## 题目描述
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `char[]` 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 $O(1)$ 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 `ASCII` 码表中的可打印字符。
### 示例
```
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```
```
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```
## 题解
### 解法一
双指针法是使用两个指针，一个左指针，右指针，开始工作时`i`指向首元素，`j`指向尾元素。交换两个指针指向的元素，并向中间移动，直到两个指针相遇。

算法：

- 将 `i` 指向首元素，`j` 指向尾元素。
- 当 `i<j`：
  - 交换 `s[i]` 和 `s[j]`。
  - `i++`
  - `j++`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200929162319.png)
```java
class Solution {
    public void reverseString(char[] s) {
        int i=0,j=s.length-1;
        while(i<j){
            char temp=s[i];
            s[i]=s[j];
            s[j]=temp;
            i++;
            j--;
        }
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$。执行了 `N/2` 次的交换。
空间复杂度：$O(1)$，只使用了常数级空间。
### 解法二
我们实现递归函数 `helper`，它接受两个参数：`left` 左指针和 `right` 右指针。

如果 `left>=right`，不做任何操作。

- 否则交换 `s[left]` 和 `s[right]` 和调用 `helper(left + 1, right - 1)`。
- 首次调用函数我们传递首尾指针反转整个字符串 `return helper(0, len(s) - 1)`。
```java
class Solution {
  public void helper(char[] s, int left, int right) {
    if (left >= right) return;
    char tmp = s[left];
    s[left++] = s[right];
    s[right--] = tmp;
    helper(s, left, right);
  }

  public void reverseString(char[] s) {
    helper(s, 0, s.length - 1);
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$。执行了 `N/2` 次的交换。
- 空间复杂度：$O(N)$，递归过程中使用的堆栈空间。

