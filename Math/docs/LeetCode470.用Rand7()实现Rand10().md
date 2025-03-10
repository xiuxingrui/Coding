# [LeetCode470.用Rand7()实现Rand10()](https://leetcode-cn.com/problems/implement-rand10-using-rand7/)
## 题目描述
已有方法 `rand7` 可生成 1 到 7 范围内的均匀随机整数，试写一个方法 `rand10` 生成 1 到 10 范围内的均匀随机整数。

不要使用系统的`Math.random()`方法。

- `rand7` 已定义。
- 传入参数: `n` 表示 `rand10` 的调用次数。

- `rand7()`调用次数的 期望值 是多少 ?
- 你能否尽量少调用 `rand7()` ?
### 示例
```
输入: 1
输出: [7]
```
```
输入: 2
输出: [8,4]
```
```
输入: 3
输出: [8,1,10]
```
## 题解
### 从 `rand10()` 到 `rand7()`
如果题目是给你 `rand10()`，让你生成 `1～7` 之间的某个数，那非常好办，我们只要不断调用 `rand10()` 即可，直到得到我们要的数，但是为什么可以呢？你可能会怀疑这个是不是等概率的，我们来计算一下

- 如果第一次就 `rand` 到 `1～7` 之间的数，那就是直接命中了，概率为 1/10
- 如果第二次命中,那么第一次必定没命中，没命中的概率为 `3/10`,再乘命中的概率`1/10`,所以第二次命中的概率是`1/10*3/10`

依次类推，我们求和，可以得到如下结果，可以知道，从 `rand10()` 到 `rand7` 它是等概率的

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200816222156.png)

譬如 `rand10`得到`rand7`

思路很简单，如果得到8-10，就继续调用，直到处于1-7为止

但是遇到 `rand50`得到`rand7` 呢？

做个取余就好1-49 映射到 1-7很容易，50就继续循环调用(范围的右边界必须是7的倍数，这样才是等概率)

你会发现这个题目的核心就是 等概率映射，映射不到的就继续调用

### 从 `rand7()` 到 `rand10()`
现在要从 `rand7()` 到 `rand10()`，也要求是等概率的，那只要我们把小的数映射到一个大的数就好办了，那首先想到的办法是乘个两倍试一试，每个 rand7()rand7()rand7() 它能生成数的范围是 `1～7`，`rand` 两次，那么数的范围就变为 `2～14`，哦，你可能发现没有 `1` 了，想要再减去个 `1` 来弥补，`rand7() + rand7()− 1`，其实这样是错误的做法，因为对于数字 5 这种，你有两种组合方式 `(2+3 or 3+2)`，而对于 `14`，你只有一种组合方式`(7+7)`，它并不是等概率的，那么简单的加减法不能使用，因为它会使得概率不一致，我们的方法是利用乘法，一般思路如下面这样构建：

$(\operatorname{rand} ()7-1) * 7+\operatorname{rand} ()7$

- 首先 `rand7()−1` 得到的数的集合为 `{0,1,2,3,4,5,6}`
- 再乘 7 后得到的集合 `A` 为 `{0，7，14，21，28，35，42}`
- 后面 `rand7()` 得到的集合B为 `{1,2,3,4,5,6,7}`

有人可能会疑惑，你咋不乘 6，乘 5 呢？因为它不是等概率生成，只有乘 7 才能使得结果是等概率生成的，啥意思？我们得到的集合 `A` 和集合 `B`，利用这两个集合，得到的数的范围是 `1～49`，每个数它显然是等概率出现的，因为这两个事件是独立事件，如果你不懂什么是独立事件，你试着加加看也能体会一点。

$P(A B)=P(A) * P(B)=\frac{1}{7} * \frac{1}{7}$

可以得到这样一个规律：
```
已知 rand_N() 可以等概率的生成[1, N]范围的随机数
那么：
(rand_X() - 1) × Y + rand_Y() ==> 可以等概率的生成[1, X * Y]范围的随机数
即实现了 rand_X*Y()
```
要实现`rand10()`，就需要先实现`rand_N()`，并且保证N大于10且是10的倍数。这样再通过`rand_N() % 10 + 1` 就可以得到`[1,10]`范围的随机数了。

那么我们可以写出下面的代码

```java
class Solution extends SolBase {
    public int rand10() {
        // 首先得到一个数
        int num = (rand7() - 1) * 7 + rand7();
        // 只要它还大于10，那就给我不断生成，因为我只要范围在1-10的，最后直接返回就可以了
        while (num > 10){
            num = (rand7() - 1) * 7 + rand7();
        }
        return num;
    }
}
```

提交发现跑的很慢

这样的一个问题是，我们的函数会得到 `1～49` 之间的数，而我们只想得到 `1～10` 之间的数，这一部分占的比例太少了，简而言之，这样效率太低，太慢，可能要 `while` 循环很多次，那么解决思路就是舍弃一部分数，舍弃 `41～49`，因为是独立事件，我们生成的 `1～40` 之间的数它是等概率的，我们最后完全可以利用 `1～40` 之间的数来得到 `1～10` 之间的数。所以，我们的代码可以改成下面这样

```java
public int rand10() {
    // 首先得到一个数
    int num = (rand7() - 1) * 7 + rand7();
    // 只要它还大于40，那你就给我不断生成吧
    while (num > 40)
        num = (rand7() - 1) * 7 + rand7();
    // 返回结果，+1是为了解决 40%10为0的情况
    return 1 + num % 10;
}
```
### 再优化
更进一步，这时候我们舍弃了 9 个数，舍弃的还是有点多，效率还是不高，怎么提高效率呢？那就是舍弃的数最好再少一点！因为这样能让 `while` 循环少转几次，那么对于大于 40 的随机数，别舍弃呀，利用这 9 个数，再利用那个公式操作一下：

`(大于40的随机数−40−1)∗7+rand7()`

这样我们可以得到 1−631-631−63 之间的随机数，只要舍弃 333 个即可，那对于这 333 个舍弃的，还可以再来一轮：

这样我们可以得到 `1−63` 之间的随机数，只要舍弃 3 个即可，那对于这 3 个舍弃的，还可以再来一轮：

`(大于60的随机数−60−1)∗7+rand7()`

这样我们可以得到 `1−21` 之间的随机数，只要舍弃 1 个即可。

```java
/**
 * The rand7() API is already defined in the parent class SolBase.
 * public int rand7();
 * @return a random integer in the range 1 to 7
 */
class Solution extends SolBase {
    public int rand10() {
        while (true){
            int num = (rand7() - 1) * 7 + rand7();
            // 如果在40以内，那就直接返回
            if(num <= 40) return 1 + num % 10;
            // 说明刚才生成的在41-49之间，利用随机数再操作一遍
            num = (num - 40 - 1) * 7 + rand7();
            if(num <= 60) return 1 + num % 10;
            // 说明刚才生成的在61-63之间，利用随机数再操作一遍
            num = (num - 60 - 1) * 7 + rand7();
            if(num <= 20) return 1 + num % 10;

        }
    }
}
```
#### 复杂度分析
- 时间复杂度：期望时间复杂度为 $O(1)$，但最坏情况下会达到 $O(∞)$(一直被拒绝)。
- 空间复杂度：$O(1)$。








