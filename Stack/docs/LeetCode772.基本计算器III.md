# [LeetCode772.基本计算器III](https://leetcode-cn.com/problems/basic-calculator-iii/)
## 题目描述
实现一个基本的计算器来计算简单的表达式字符串。

表达式字符串只包含非负整数， `+`, `-`, `*`, `/` 操作符，左括号 `(` ，右括号 `)`和空格` `。整数除法需要向下截断。

你可以假定给定的字符串总是有效的。所有的中间结果的范围为 `[-2147483648, 2147483647]`。

进阶：你可以在不使用内置库函数的情况下解决此问题吗？

- `1 <= s <= 104`
- `s` 由整数、`'+'`、`'-'`、`'*'`、`'/'`、`'('`、`')'` 和 `' '` 组成
- `s` 是一个 有效的 表达式

### 示例
```
输入：s = "1 + 1"
输出：2
```
```
输入：s = " 6-4 / 2 "
输出：4
```
```
输入：s = "2*(5+5*2)/3+(6/2+8)"
输出：21
```
```
输入：s = "(2+6* 3+5- (3*14/7+2)*5)+3"
输出：-12
```
```
输入：s = "0"
输出：0
```
## 题解
我们只需要把题目给的中缀表达式转成后缀表达式，直接调用上边计算逆波兰式就可以了。

中缀表达式转后缀表达式也有一个[通用的方法](https://blog.csdn.net/sgbfblog/article/details/8001651)。

1. 如果遇到操作数，我们就直接将其加入到后缀表达式。
2. 如果遇到左括号，则我们将其放入到栈中。
3. 如果遇到一个右括号，则将栈元素弹出，将弹出的操作符加入到后缀表达式直到遇到左括号为止，接着将左括号弹出，但不加入到结果中。
4. 如果遇到其他的操作符，如（`“+”`， `“-”`）等，从栈中弹出元素将其加入到后缀表达式，直到栈顶的元素优先级比当前的优先级低（或者遇到左括号或者栈为空）为止。弹出完这些元素后，最后将当前遇到的操作符压入到栈中。
5. 如果我们读到了输入的末尾，则将栈中所有元素依次弹出。

然后就是对数字的处理，因为数字可能并不只有一位，所以遇到数字的时候要不停的累加。

当遇到运算符或者括号的时候就将累加的数字加到后缀表达式中。

唯一需要注意的地方就是计算的中间结果范围可能出现超出 int 类型的情况。因此，在进行逆波兰计算的时候，使用 `long` 来计算。
```java
class Solution {
    public int calculate(String s) {
        String[] polish=getPolish(s);
        Deque<Integer> stack = new LinkedList<>();
        for (String item : polish) {
            if (item.equals("+")) {
                stack.push(stack.pop() + stack.pop());
            } else if (item.equals("-")) {
                int op2=stack.pop();
                int op1=stack.pop();
                stack.push(op1-op2);
            } else if (item.equals("*")) {
                stack.push(stack.pop() * stack.pop());
            } else if (item.equals("/")) {
                int op2=stack.pop();
                int op1=stack.pop();
                stack.push(op1/op2);
            } else {
                stack.push(Integer.parseInt(item));
            }
        }
        return stack.pop();
    }
    public String[] getPolish(String s){
        List<String> res=new ArrayList<>();
        Deque<String> stack=new LinkedList<>();
        int num=-1,len=s.length();
        char[] arr=s.toCharArray();
        for(char c:arr){
            if(c==' '){
                continue;
            }
            if(Character.isDigit(c)){
                if(num==-1){
                    num=c-'0';
                }else{
                    num=num*10+c-'0';
                }
            }else{
                if(num!=-1){
                    res.add(num+"");
                    num=-1;
                }
                if(c=='+'||c=='-'){
                    while(!stack.isEmpty()&&!stack.peek().equals("(")){
                        res.add(stack.pop());
                    }
                    stack.push(c+"");
                }else if(c=='*'||c=='/'){
                    while(!stack.isEmpty()&&!stack.peek().equals("(")&&(stack.peek().equals("*")||stack.peek().equals("/"))){
                        res.add(stack.pop());
                    }
                    stack.push(c+"");
                }else if(c=='('){
                    stack.push("(");
                }else{
                    while(!stack.isEmpty()&&!stack.peek().equals("(")){
                        res.add(stack.pop());
                    }
                    stack.pop();
                }
            }
        }
        if(num!=-1){
            res.add(num+"");
        }
        while(!stack.isEmpty()){
            res.add(stack.pop());
        }
        String[] ans = new String[res.size()];
        for (int i = 0; i < res.size(); i++) {
            ans[i] = res.get(i);
        }
        return ans;
    }
}
```