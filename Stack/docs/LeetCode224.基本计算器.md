# [LeetCode224.基本计算器](https://leetcode-cn.com/problems/basic-calculator/)
## 题目描述
实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式可以包含左括号 `(` ，右括号 `)`，加号 `+` ，减号 `-`，非负整数和空格`  `。

### 示例
```
输入: "1 + 1"
输出: 2
```
```
输入: " 2-1 + 2 "
输出: 3
```
```
输入: "(1+(4+5+2)-3)+(6+8)"
输出: 23
```
## 题解
我们只需要把题目给的中缀表达式转成后缀表达式，直接调用上边计算逆波兰式就可以了。

中缀表达式转后缀表达式也有一个[通用的方法](https://blog.csdn.net/sgbfblog/article/details/8001651)。

1. 如果遇到操作数，我们就直接将其加入到后缀表达式。
2. 如果遇到左括号，则我们将其放入到栈中。
3. 如果遇到一个右括号，则将栈元素弹出，将弹出的操作符加入到后缀表达式直到遇到左括号为止，接着将左括号弹出，但不加入到结果中。
4. 如果遇到其他的操作符，如（`“+”`， `“-”`）等，从栈中弹出元素将其加入到后缀表达式，直到栈顶的元素优先级比当前的优先级低（或者遇到左括号或者栈为空）为止。弹出完这些元素后，最后将当前遇到的操作符压入到栈中。
5. 如果我们读到了输入的末尾，则将栈中所有元素依次弹出。

这里的话注意一下第四条规则，因为题目中只有加法和减法，加法和减法是同优先级的，所以一定不会遇到更低优先级的元素，所以「直到栈顶的元素优先级比当前的优先级低（或者遇到左括号或者栈为空）为止。」这句话可以改成「直到遇到左括号或者栈为空为止」。

然后就是对数字的处理，因为数字可能并不只有一位，所以遇到数字的时候要不停的累加。

当遇到运算符或者括号的时候就将累加的数字加到后缀表达式中。

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