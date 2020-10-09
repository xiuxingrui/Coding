# [LeetCode43.字符串相乘](https://leetcode-cn.com/problems/multiply-strings/)
## 题目描述
给定两个以字符串形式表示的非负整数 `num1` 和 `num2`，返回 `num1` 和 `num2` 的乘积，它们的乘积也表示为字符串形式。

说明：

- `num1` 和 `num2` 的长度小于110。
- `num1` 和 `num2` 只包含数字 0-9。
- `num1` 和 `num2` 均不以零开头，除非是数字 0 本身。
- 不能使用任何标准库的大数类型（比如 `BigInteger`）或直接将输入转换为整数来处理。

### 示例
```
输入: num1 = "2", num2 = "3"
输出: "6"
```
```
输入: num1 = "123", num2 = "456"
输出: "56088"
```
## 题解
### 解法一
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009183003.png)

```java
class Solution {
    public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) {
            return "0";
        }
        int m = num1.length(), n = num2.length();
        int[] ansArr = new int[m + n];
        for (int i = m - 1; i >= 0; i--) {
            int x = num1.charAt(i) - '0';
            for (int j = n - 1; j >= 0; j--) {
                int y = num2.charAt(j) - '0';
                ansArr[i + j + 1] += x * y;
            }
        }
        for (int i = m + n - 1; i > 0; i--) {
            ansArr[i - 1] += ansArr[i] / 10;
            ansArr[i] %= 10;
        }
        int index = ansArr[0] == 0 ? 1 : 0;
        StringBuffer ans = new StringBuffer();
        while (index < m + n) {
            ans.append(ansArr[index]);
            index++;
        }
        return ans.toString();
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(mn)$，其中 `m` 和 `n` 分别是 `num1`和 `num2`的长度。需要计算 `num1`的每一位和 `num2`的每一位的乘积。
- 空间复杂度：$O(m+n)$，其中 `m` 和 `n` 分别是 `num1`和 `num2`的长度。需要创建一个长度为 `m+n` 的数组存储乘积。


### 解法二
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009180617.png)

自己的解法：
```java
class Solution {
    public String multiply(String num1, String num2) {
        if(num1.equals("0")||num2.equals("0")){
            return "0";
        }
        int len1=num1.length(),len2=num2.length();
        char[] nums1=num1.toCharArray();
        char[] nums2=num2.toCharArray();
        int[] nums=new int[len1+len2];
        int idx=0;
        for(int i=len1-1;i>=0;i--){
            idx=len1-1-i;
            int n1=nums1[i]-'0';
            for(int j=len2-1;j>=0;j--){
                int n2=nums2[j]-'0';
                nums[idx++]+=n1*n2;
            }
        }
        int i=0,flag=0;
        while(i<idx||flag!=0){
            int num=nums[i]+flag;
            nums[i]=num%10;
            flag=num/10;
            i++;
        }
        StringBuilder sb=new StringBuilder();
        int len=i;
        for(i=0;i<len;i++){
            sb.append(nums[i]);
        }
        sb.reverse();
        return sb.toString();
    }
}
```