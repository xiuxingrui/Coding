# [剑指Offer49.丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)
## 题目描述
我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 `n` 个丑数。

- 1 是丑数。
- `n` 不超过1690。
### 示例
```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```
## 题解
### 解法一
丑数的递推性质： 丑数只包含因子 2,3,5，因此有 “丑数 === 某较小丑数 × 某因子” （例如：10=5×2）。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007201339.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007201416.png)

状态定义： 设动态规划列表 `dp` ，`dp[i]` 代表第 `i+1` 个丑数。

转移方程：

- 当索引 `a,b,c` 满足以下条件时， `dp[i]` 为三种情况的最小值；
- 每轮计算 `dp[i]` 后，需要更新索引 `a,b,c` 的值，使其始终满足方程条件。实现方法：分别**独立判断**(防止出现`2*3`和`3*2`的重复解) `dp[i]` 和 `dp[a]×2`, `dp[b]×3`, `dp[c]×5` 的大小关系，若相等则将对应索引 `a,b,c` 加 1。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007201647.png)

初始状态： `dp[0]=1` ，即第一个丑数为 1 ；

返回值： `dp[n−1]` ，即返回第 `n` 个丑数。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007202248.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007202259.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007202309.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007202321.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007202333.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201007202342.png)
```java
// 一个十分巧妙的动态规划问题
// 1.我们将前面求得的丑数记录下来，后面的丑数就是前面的丑数*2，*3，*5
// 2.但是问题来了，我怎么确定已知前面k-1个丑数，我怎么确定第k个丑数呢
// 3.采取用三个指针的方法，p2,p3,p5
// 4.index2指向的数字下一次永远*2，p3指向的数字下一次永远*3，p5指向的数字永远*5
// 5.我们从2*p2 3*p3 5*p5选取最小的一个数字，作为第k个丑数
// 6.如果第K个丑数==2*p2，也就是说前面0-p2个丑数*2不可能产生比第K个丑数更大的丑数了，所以p2++
// 7.p3,p5同理
// 8.返回第n个丑数

class Solution {
    public int nthUglyNumber(int n) {
        int a = 0, b = 0, c = 0;
        int[] dp = new int[n];
        dp[0] = 1;
        for(int i = 1; i < n; i++) {
            int n2 = dp[a] * 2, n3 = dp[b] * 3, n5 = dp[c] * 5;
            dp[i] = Math.min(Math.min(n2, n3), n5);
            if(dp[i] == n2) a++;
            if(dp[i] == n3) b++;
            if(dp[i] == n5) c++;
        }
        return dp[n - 1];
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(N)$ ： 其中 `N=n` ，动态规划需遍历计算 `dp` 列表。
- 空间复杂度 $O(N)$： 长度为 `N` 的 `dp` 列表使用 $O(N)$ 的额外空间。

### 解法三
用`TreeSet`，既去重又排序,相当于优先队列
```java
class Solution {
    public int nthUglyNumber(int n) {
        TreeSet<Long> set = new TreeSet<Long>();
        int count = 0;
        long result = 1;
        set.add(result);
        while (count < n) {
            result = set.pollFirst();
            count++;
            set.add(result * 2);
            set.add(result * 3);
            set.add(result * 5);
        }
        return (int) result;
    }
}
```
或：
```java
class Solution {
    public int nthUglyNumber(int n) {
        PriorityQueue<Long> pq = new PriorityQueue<>();
        Set<Long> s = new HashSet<>();
        //初始化,放进堆和set，发现1要开Long数组才可以
        long[] primes = new long[]{2, 3, 5};
        long num=1;
        pq.offer(num);
        for (int i = 1; i <=n; i++) {
            num = pq.poll();
            //遍历三个因子
            for (int j = 0; j < 3; j++) {
                if (!s.contains(num * primes[j])) {
                    pq.offer(num * primes[j]);
                    s.add(num * primes[j]);
                }
            }
        }
        return (int) num;
    }
}
```
或
```java
public int nthUglyNumber(int n) {
    Queue<Long> queue = new PriorityQueue<Long>();
    int count = 0;
    long result = 1;
    queue.add(result);
    while (count < n) {
        result = queue.poll();
        // 删除重复的
        while (!queue.isEmpty() && result == queue.peek()) {
            queue.poll();
        }
        count++;
        queue.offer(result * 2);
        queue.offer(result * 3);
        queue.offer(result * 5);
    }
    return (int) result;
}
```