# [LeetCode65.有效数字](https://leetcode-cn.com/problems/valid-number/)
## 题目描述
验证给定的字符串是否可以解释为十进制数字。

例如:

- `"0" => true`
- `" 0.1 " => true`
- `"abc" => false`
- `"1 a" => false`
- `"2e10" => true`
- `" -90e3   " => true`
- `" 1e" => false`
- `"e3" => false`
- `" 6e-1" => true`
- `" 99e2.5 " => false`
- `"53.5e93" => true`
- `" --6 " => false`
- `"-+3" => false`
- `"95a54e53" => false`

说明: 我们有意将问题陈述地比较模糊。在实现代码之前，你应当事先思考所有可能的情况。这里给出一份可能存在于有效十进制数字中的字符列表：

- 数字 0-9
- 指数 `- "e"`
- 正/负号 `- "+"/"-"`
- 小数点 `- "."`

当然，在输入中，这些字符的上下文也很重要。

## 题解
易错的用例：

- `true ：".1" "-.1" "1." " 005047e+6""46.e3" ".2e81"`
- `false："." "+e" "+3. e04116" "+3. e04116"`

先设定`numSeen`，`dotSeen`和`eSeen`三种`boolean`变量，分别代表是否出现数字、点和`E`
然后遍历目标字符串
1. 判断是否属于数字的0~9区间
2. 遇到点的时候，判断前面是否有点或者`E`，都需要`return false`
3. 遇到`E`的时候，判断前面数字是否合理，是否有`E`，并把`numSeen`置为`false`，防止`E`后无数字
4. 遇到`-+`的时候，判断是否是第一个，如果不是第一个判断是否在`E`后面，都不满足则`return false`
5. 其他情况都为`false`
最后返回`numSeen`的结果即可

```java
class Solution {
    public boolean isNumber(String s) {
        if(s==null||s.length()==0) return false;
        boolean numSeen=false;
        boolean dotSeen=false;
        boolean eSeen=false;
        char arr[]=s.trim().toCharArray();
        for(int i=0; i<arr.length; i++){
            if(arr[i]>='0'&&arr[i]<='9'){
                numSeen=true;
            }else if(arr[i]=='.'){
                if(dotSeen||eSeen){
                    return false;
                }
                dotSeen=true;
            }else if(arr[i]=='E'||arr[i]=='e'){
                if(eSeen||!numSeen){
                    return false;
                }
                eSeen=true;
                numSeen=false;
            }else if(arr[i]=='+'||arr[i]=='-'){
                if(i!=0&&arr[i-1]!='e'&&arr[i-1]!='E'){
                    return false;
                }
            }else{
                return false;
            }
        }
        return numSeen;
    }
}
```
