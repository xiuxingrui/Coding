# [剑指Offer62.圆圈中最后剩下的数字(约瑟夫环)](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)
## 题目描述
`0,1,...,n-1`这`n`个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第`m`个数字。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

- `1 <= n <= 10^5`
- `1 <= m <= 10^6`
### 示例
```
输入: n = 5, m = 3
输出: 3
```
```
输入: n = 10, m = 17
输出: 2
```

## 题解
### 解法一:模拟
假设当前删除的位置是 `idx`，下一个删除的数字的位置是 `idx+m`。但是，由于把当前位置的数字删除了，后面的数字会前移一位，所以实际的下一个位置是 `idx+m−1`。由于数到末尾会从头继续数，所以最后取模一下，就是 $(idx+m-1)mod(n)$。

至于这种思路的代码实现，`LinkedList` 会超时，是因为 `LinkedList` 虽然删除指定节点的时间复杂度是 $O(1)$ 的，但是在 `remove` 时间复杂度仍然是$O(n)$ 的，因为需要从头遍历到需要删除的位置。那 `ArrayList` 呢？索引到需要删除的位置，时间复杂度是 $O(1)$，删除元素时间复杂度是 $O(n)$（因为后续元素需要向前移位）， `remove` 整体时间复杂度是 $O(n)$ 的。看起来`LinkedList` 和 `ArrayList` 单次删除操作的时间复杂度是一样的 ？`ArrayList` 的 `remove` 操作在后续移位的时候，其实是内存连续空间的拷贝的！所以相比于`LinkedList`大量非连续性地址访问，`ArrayList`的性能是很 OK 的！

```java
class Solution {
    public int lastRemaining(int n, int m) {
        ArrayList<Integer> list = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            list.add(i);
        }
        int idx = 0;
        while (n > 1) {
            idx = (idx + m - 1) % n;
            list.remove(idx);
            n--;
        }
        return list.get(0);
    }
}
```
### 解法二:数学
我们给每个人一个编号（索引值），每个人用字母代替，下面这个例子是`N=8`, `m=3`的例子

我们定义`F(n,m)`表示最后剩下那个人的索引号，因此我们只关系最后剩下来这个人的索引号的变化情况即可

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200801124132.png)

从8个人开始，每次杀掉一个人，去掉被杀的人，然后把杀掉那个人之后的第一个人作为开头重新编号

- 第一次`C`被杀掉，人数变成7，`D`作为开头，（最终活下来的`G`的编号从6变成3）
- 第二次`F`被杀掉，人数变成6，`G`作为开头，（最终活下来的`G`的编号从3变成0）
- 第三次`A`被杀掉，人数变成5，`B`作为开头，（最终活下来的`G`的编号从0变成3）
- 以此类推，当只剩一个人时，他的编号必定为0！（重点！）

现在我们知道了`G`的索引号的变化过程，那么我们反推一下从`N = 7` 到`N = 8` 的过程

如何才能将`N = 7` 的排列变回到`N = 8` 呢？

我们先把被杀掉的`C`补充回来，然后右移`m`个人，发现溢出了，再把溢出的补充在最前面

经过这个操作就恢复了`N = 8` 的排列了！

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200801124336.png)

因此我们可以推出递推公式`f(8,3)=[f(7,3)+3]%8`

进行推广泛化，即$f(n,m)=[f(n−1,m)+m]%n$

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200801124551.png)

```java
class Solution {
    public int lastRemaining(int n, int m) {
        int ans=0;
        for(int i=2;i<=n;i++){
            ans=(ans+m)%i;
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，需要求解的函数值有 `n` 个。
- 空间复杂度：$O(1)$，只使用常数个变量。


