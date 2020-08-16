# [LeetCode458.可怜的小猪](https://leetcode-cn.com/problems/poor-pigs/)
## 题目描述
有 1000 只水桶，其中有且只有一桶装的含有毒药，其余装的都是水。它们从外观看起来都一样。如果小猪喝了毒药，它会在 15 分钟内死去。

问题来了，如果需要你在一小时内，弄清楚哪只水桶含有毒药，你最少需要多少只猪？

回答这个问题，并为下列的进阶问题编写一个通用算法。

 
假设有 `n` 只水桶，猪饮水中毒后会在 `m` 分钟内死亡，你需要多少猪（`x`）就能在 `p` 分钟内找出 “有毒” 水桶？这 `n` 只水桶里有且仅有一只有毒的桶。

- 可以允许小猪同时饮用任意数量的桶中的水，并且该过程不需要时间。
- 小猪喝完水后，必须有 `m` 分钟的冷却时间。在这段时间里，只允许观察，而不允许继续饮水。
- 任何给定的桶都可以无限次采样（无限数量的猪）。

### 扩展延伸:老鼠和毒药
实验室有100个瓶子，其中有一瓶装有慢性毒药（第3天发作)，另外99瓶装有蒸馏水。请问至少需要多少只小白鼠才能在3天内找出哪一瓶是慢性毒药？

利用二进制来做，最少的老鼠数量就是计算2的多少次方大于等于瓶子数量，例如本题为7。对100瓶进行二进制编码，这样可以排列出`1xxxxxx`，`x1xxxxxx`，...，`xxxxxx1`这样的七组序列，如果是`1xxxxxx`和`x1xxxxx`的老鼠死了，表示`1100000`有毒。

## 题解
举例说明：

- 假设：总时间 `minutesToTest = 60`，死亡时间 `minutesToDie = 15`，`pow(x, y)` 表示 `x` 的 `y` 次方，`ceil(x)`表示 `x` 向上取整
- 当前有 1 只小猪，最多可以喝 `times = minutesToTest / minutesToDie = 4` 次水
- 最多可以喝 4 次水，能够携带 `base = times + 1 = 5` 个的信息量，也就是（便于理解从 0 开始）：
  - (1) 喝 0 号死去，0 号桶水有毒
  - (2) 喝 1 号死去，1 号桶水有毒
  - (3) 喝 2 号死去，2 号桶水有毒
  - (4) 喝 3 号死去，3 号桶水有毒
  - (5) 喝了上述所有水依然活蹦乱跳，4 号桶水有毒
结论是 1 只小猪最多能够验证 5 桶水中哪只水桶含有毒药，当 `buckets ≤ 5` 时，`answer = 1`

那么 2 只小猪可以验证的范围最多到多少呢？我们把每只小猪携带的信息量看成是 `base`进制数，2 只小猪的信息量就是 `pow(base, 2) = pow(5, 2) = 25`，所以当 `5 ＜ buckets ≤ 25`时，`anwser = 2`

那么可以得到公式关系：`pow(base, ans) ≥ buckets`，取对数后即为：`ans ≥ log(buckets) / log(base)`，因为 `ans` 为整数，所以 `ans = ceil(log(buckets) / log(base))`

时间复杂度：$O(1)$

看到这里我们再去关注细节，2 只小猪到底怎么喂水，在上述假设下，能够最多验证 25 桶水呢？请看下方图画解答：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200817010552.png)

第一只猪喝列的水的混合物，第二只猪喝行的水的混合物，最后一行和一列不喝。

```java
class Solution {
    public int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int times = minutesToTest / minutesToDie;
        int base = times + 1;
        // base ^ ans >= buckets 
        // ans >= log(buckets) / log(base)
        double temp = Math.log(buckets) / Math.log(base);
        int ans = (int)Math.ceil(temp)
        return ans;
    }
}
```




 