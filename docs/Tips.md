# Tips
## 集合及核心类
- [集合](集合.md)
- [核心类](核心类.md)
## 输入输出
记得要先`import`:
```java
import java.util.*;
import java.util.Scanner;
```
`java.util.Scanner` 是 `Java5` 的新特征，我们可以通过 `Scanner` 类来获取用户的输入。

下面是创建 `Scanner` 对象的基本语法：
```java
Scanner s = new Scanner(System.in);
```
接下来我们演示一个最简单的数据输入，并通过 `Scanner` 类的 `next()` 与 `nextLine()` 方法获取输入的字符串，在读取前我们一般需要 使用 `hasNext` 与 `hasNextLine` 判断是否还有输入的数据：

```java
import java.util.Scanner; 
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
 
        // next方式接收字符串
        System.out.println("next方式接收：");
        // 判断是否还有输入
        if (scan.hasNext()) {
            String str1 = scan.next();
            System.out.println("输入的数据为：" + str1);
        }
        scan.close();
    }
}
```
执行以上程序输出结果为：
```
next方式接收：
runoob com
输入的数据为：runoob
```
可以看到 `com` 字符串并未输出，接下来我们看 `nextLine`。

```java
import java.util.Scanner;
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
 
        // nextLine方式接收字符串
        System.out.println("nextLine方式接收：");
        // 判断是否还有输入
        if (scan.hasNextLine()) {
            String str2 = scan.nextLine();
            System.out.println("输入的数据为：" + str2);
        }
        scan.close();
    }
}
```
执行以上程序输出结果为：
```
nextLine方式接收：
runoob com
输入的数据为：runoob com
```
可以看到 `com` 字符串输出。

`next()` 与 `nextLine()` 区别

`next()`:
1. 一定要读取到有效字符后才可以结束输入。
2. 对输入有效字符之前遇到的空白，`next()` 方法会自动将其去掉。
3. 只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
4. `next()` 不能得到带有空格的字符串。
`nextLine()`：
1. 以`Enter`为结束符,也就是说 `nextLine()`方法返回的是输入回车之前的所有字符。
2. 可以获得空白。

如果要输入 `int` 或 `float` 类型的数据，在 `Scanner` 类中也有支持，但是在输入之前最好先使用 `hasNextXxx()` 方法进行验证，再使用 `nextXxx()` 来读取：
```java
import java.util.Scanner;
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
        int i = 0;
        float f = 0.0f;
        System.out.print("输入整数：");
        if (scan.hasNextInt()) {
            // 判断输入的是否是整数
            i = scan.nextInt();
            // 接收整数
            System.out.println("整数数据：" + i);
        } else {
            // 输入错误的信息
            System.out.println("输入的不是整数！");
        }
        System.out.print("输入小数：");
        if (scan.hasNextFloat()) {
            // 判断输入的是否是小数
            f = scan.nextFloat();
            // 接收小数
            System.out.println("小数数据：" + f);
        } else {
            // 输入错误的信息
            System.out.println("输入的不是小数！");
        }
        scan.close();
    }
}
```
执行以上程序输出结果为：
```
输入整数：12
整数数据：12
输入小数：1.2
小数数据：1.2
```
以下实例我们可以输入多个数字，并求其总和与平均数，每输入一个数字用回车确认，通过输入非数字来结束输入并输出执行结果：
```java
import java.util.Scanner;
 
class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
 
        double sum = 0;
        int m = 0;
 
        while (scan.hasNextDouble()) {
            double x = scan.nextDouble();
            m = m + 1;
            sum = sum + x;
        }
 
        System.out.println(m + "个数的和为" + sum);
        System.out.println(m + "个数的平均值是" + (sum / m));
        scan.close();
    }
}
```
执行以上程序输出结果为：
```
12
23
15
21.4
end
4个数的和为71.4
4个数的平均值是17.85
```
廖雪峰：
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        System.out.print("Input your name: "); // 打印提示
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        System.out.print("Input your age: "); // 打印提示
        int age = scanner.nextInt(); // 读取一行输入并获取整数
        System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出
    }
}
```
创建`Scanner`对象并传入`System.in`。`System.out`代表标准输出流，而`System.in`代表标准输入流。直接使用`System.in`读取用户输入虽然是可以的，但需要更复杂的代码，而通过`Scanner`就可以简化后续的代码。

处理循环输入：
```java
Scanner scan = new Scanner(System.in);
while(scan.hasNext()){
    String str = scan.nextLine();
    //处理str的函数
}
```
比如输入
```
3
2
2
1
11
10
20
40
32
67
40
20
89
300
400
15
```
样例有两组测试
第一组是3个数字，分别是：2，2，1。
第二组是11个数字，分别是：10，20，40，32，67，40，20，89，300，400，15。
```java
import java.util.Scanner;
import java.util.TreeSet;
 
public class Main
{
    public static void main(String[] args) {
        Scanner sc=new Scanner(System.in);
        while(sc.hasNext()){
            HashSet<Integer> set=new HashSet<>();
            int n=sc.nextInt();
            if(n>0){
                for(int i=0;i<n;i++){
                    set.add(sc.nextInt());
                }
            }
            for(Integer i:set){
                System.out.println(i);
            }
        }
    }
}
```
输入一维数组：

第一行一个整数`N`，表示数组长度
第二行`N`个整数，分别表示数组内的元素

```
7
5 5 3 2 6 4 3
```
```java
Scanner scanner = new Scanner(System.in);
int N = scanner.nextInt();
int[] arr = new int[N];
for (int i = 0; i < N; i++) {
    arr[i] = scanner.nextInt();
}
```
输入二维数组：

如：实现一个函数，判断`K`是否在`matrix`中

第一行有三个整数`N`, `M`, `K`,接下来`N`行，每行`M`个整数为输入的矩阵
```
2 4 5
1 2 3 4
2 4 5 6
```
```java
Scanner sc = new Scanner(System.in);
int n = sc.nextInt();
int m = sc.nextInt();
int k = sc.nextInt();
int a[][] = new int[n][m];
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        a[i][j] = sc.nextInt();
    }
}
```
输入链表：

将节点值存入数组后建链表：
```java
//Definition for singly-linked list.
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
public ListNode stringToListNode(int[] nodeValues) {
    ListNode dummyRoot = new ListNode(0);
    ListNode ptr = dummyRoot;
    for(int item : nodeValues) {
        ptr.next = new ListNode(item);
        ptr = ptr.next;
    }
    return dummyRoot.next;
}
```
输入树：

可以参考`LeetCode`中的二叉树的序列化和反序列化：
```java
public static TreeNode stringToTreeNode(String input) {
        input = input.trim();
        input = input.substring(1, input.length() - 1);//比如去掉数组两端的'{'和’}‘
        if (input.length() == 0) {
            return null;
        }
    
        String[] parts = input.split(",");
        String item = parts[0];
        TreeNode root = new TreeNode(Integer.parseInt(item));
        Queue<TreeNode> nodeQueue = new LinkedList<>();
        nodeQueue.add(root);
    
        int index = 1;
        while(!nodeQueue.isEmpty()) {
            TreeNode node = nodeQueue.remove();
    
            if (index == parts.length) {//可判断可不判断
                break;
            }
    
            item = parts[index++];
            item = item.trim();
            if (!item.equals("null")) {
                int leftNumber = Integer.parseInt(item);
                node.left = new TreeNode(leftNumber);
                nodeQueue.add(node.left);
            }
    
            if (index == parts.length) {//可判断可不判断
                break;
            }

            item = parts[index++];
            item = item.trim();
            if (!item.equals("null")) {
                int rightNumber = Integer.parseInt(item);
                node.right = new TreeNode(rightNumber);
                nodeQueue.add(node.right);
            }
        }
        return root;
    }
```
## 其他
- 重写`Comparator`时，`compare`函数中的参数要包装类型，比如是`(Integer a,Integer b)`而非`(int a,int b)`;
- 重写`Comparator`的Lambda表达式版本：
```java
Collections.sort(list, (x, y) ->x-y);
```
```java
Collections.sort(list, (x, y) -> {
    return x - y;
});
```
- 在数组的指定下标插入元素：
```java
//people为二维数组，p[1]为数组下标
List<int[]> list=new LinkedList<>();
for(int[] p:people){
    list.add(p[1],p);
}
return list.toArray(new int[list.size()][2]);
```
- 初始化列表加入元素
```java
List<Integer> list = new ArrayList<>() {{
    add(1);
    add(2);
    }
};
```