# [LeetCode6.Z字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)
## 题目描述
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 `Z` 字形排列。

比如输入字符串为 `"LEETCODEISHIRING"` 行数为 3 时，排列如下：

```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"LCIRETOESIIGEDHN"`。

请你实现这个将字符串进行指定行数变换的函数：
```
string convert(string s, int numRows);
```
### 示例
```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```
```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G
```
## 题解
### 解法一
1. 字符串 `s` 是以 `Z` 字形为顺序存储的字符串，目标是按行打印。
2. 设 `numRows` 行字符串分别为$s_{1}, s_{2}, \ldots, s_{n}$，则容易发现：按顺序遍历字符串 `s` 时，每个字符 `c` 在 `Z` 字形中对应的 行索引 先从$s_{1}$增大至${s}_{n}$，再从${s}_{n}$减小至$s_{1}$…… 如此反复。
3. 因此，解决方案为：模拟这个行索引的变化，在遍历 `s` 中把每个字符填到正确的行 `res[i]` 。

按顺序遍历字符串 `s`；

1. `res[i] += c`： 把每个字符 `c` 填入对应行${s}_{i}$；
2. `i += flag`： 更新当前字符 `c` 对应的行索引；
3. `flag = - flag`： 在达到 `Z` 字形转折点时，执行反向。
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914162200.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914162210.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914162225.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200914162237.png)
```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows < 2) return s;
        List<StringBuilder> rows = new ArrayList<StringBuilder>();
        for(int i = 0; i < numRows; i++) rows.add(new StringBuilder());
        int i = 0, flag = -1;
        for(char c : s.toCharArray()) {
            rows.get(i).append(c);
            if(i == 0 || i == numRows -1) flag = - flag;
            i += flag;
        }
        StringBuilder res = new StringBuilder();
        for(StringBuilder row : rows) res.append(row);
        return res.toString();
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(N)$ ：遍历一遍字符串 `s`；
- 空间复杂度 $O(N)$ ：各行字符串共占用 $O(N)$ 额外空间。

### 解法二
自己的解法
```java
class Solution {
    public String convert(String s, int numRows) {
        if(s==null){
            return null;
        }
        if(s.length()==0||numRows==1){
            return s;
        }
        int len=s.length();
        char[] chs=s.toCharArray();
        char[][] temp=new char[numRows][len];
        for(int i=0;i<numRows;i++){
            Arrays.fill(temp[i],' ');
        }
        int idx=0;
        int i=0,j=0;
        while(idx<len){
            if(i==0){
                while(i<numRows&&idx<len){
                    temp[i][j]=chs[idx];
                    i++;
                    idx++;
                }
            }
            if(i==numRows){
                j++;
                i=numRows-2;
                while(i>0&&idx<len){
                    temp[i][j]=chs[idx];
                    i--;
                    j++;
                    idx++;
                }
            }
        }
        StringBuilder sb=new StringBuilder();
        for(int m=0;m<numRows;m++){
            for(int k=0;k<=j;k++){
                if(temp[m][k]!=' '){
                    sb.append(temp[m][k]);
                }
            }
        }
        return sb.toString();
    }
}
```