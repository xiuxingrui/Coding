# [LeetCode914.卡牌分组](https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/)
## 题目描述
给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 `X`，使我们可以将整副牌按下述规则分成 1 组或更多组：

- 每组都有 `X` 张牌。
- 组内所有的牌上都写着相同的整数。

仅当你可选的 `X >= 2` 时返回 `true`。

- `1 <= deck.length <= 10000`
- `0 <= deck[i] < 10000`
## 示例
```
输入：[1,2,3,4,4,3,2,1]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]
```
```
输入：[1,1,1,2,2,2,3,3]
输出：false
解释：没有满足要求的分组。
```
```
输入：[1]
输出：false
解释：没有满足要求的分组。
```
```
输入：[1,1]
输出：true
解释：可行的分组是 [1,1]
```
```
输入：[1,1,2,2,2,2]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[2,2]
```
## 题解
### 解法一
`X`一定为 `count[i]`的约数，这个条件是对所有牌中存在的数字 `i` 成立的，所以我们可以推出，只有当 `X` 为所有 `count[i]`的约数，即所有 `count[i]`的最大公约数的约数时，才存在可能的分组。公式化来说，我们假设牌中存在的数字集合为 `a, b, c, d, e`，那么只有当 `X` 为

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201006204007.png)

的约数时才能满足要求。

因此我们只要求出所有 `count[i]`最大公约数 `g`，判断 `g` 是否大于等于 2 即可，如果大于等于 2，则满足条件，否则不满足。
```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        int[] count = new int[10000];
        for (int c: deck)
            count[c]++;

        int g = -1;
        for (int i = 0; i < 10000; ++i)
            if (count[i] > 0) {
                if (g == -1)
                    g = count[i];
                else
                    g = gcd(g, count[i]);
            }

        return g >= 2;
    }

    public int gcd(int x, int y) {
        return y == 0 ? x : gcd(y,x%y);
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(NlogC)$，其中 `N` 是卡牌的个数，`C` 是数组 `deck` 中数的范围，在本题中 `C` 的值为 10000。求两个数最大公约数的复杂度是 $O(logC)$，需要求最多 `N−1` 次。
- 空间复杂度：$O(N+C)$ 或 $O(N)$。

### 解法二
我们枚举所有可行的 `X`，判断是否有满足条件的 `X` 即可。

我们从 2 开始，从小到大枚举 `X`。

由于每一组都有 `X` 张牌，那么 `X` 必须是卡牌总数 `N` 的约数。

其次，对于写着数字 `i` 的牌，如果有 `count[i]`张，由于题目要求「组内所有的牌上都写着相同的整数」，那么 `X` 也必须是 `count[i]`的约数，即：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201006204955.png)

所以对于每一个枚举到的 `X`，我们只要先判断 `X` 是否为 `N` 的约数，然后遍历所有牌中存在的数字 `i`，看它们对应牌的数量 `count[i]`是否满足上述要求。如果都满足等式，则 `X` 为符合条件的解，否则需要继续令 `X` 增大，枚举下一个数字。

```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        int N = deck.length;
        int[] count = new int[10000];
        for (int c: deck)
            count[c]++;

        List<Integer> values = new ArrayList();
        for (int i = 0; i < 10000; ++i)
            if (count[i] > 0)
                values.add(count[i]);

        search: for (int X = 2; X <= N; ++X)
            if (N % X == 0) {
                boolean flag=false;
                for (int v: values){
                    if (v % X != 0){
                        flag=true;
                        break;
                    }
                }
                if(flag){
                    continue;
                }
                return true;
            }

        return false;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N^2)$，其中 `N` 是卡牌个数。最多枚举 `N` 个可能的 `X`，对于每个 `X`，要遍历的数字 `i` 最多有 `N` 个。
- 空间复杂度：$O(N+C)$ 或 $O(N)$，其中 `C` 是数组 `deck` 中数的范围，在本题中 `C` 的值为 10000。在 `Java` 代码中，我们先用频率数组 `count` 存储了每个数字 `i` 出现的次数 `count[i]`，随后将所有超过零的次数转移到数组 `values` 中，方便进行遍历，因此需要使用长度为 `C` 的 `count` 数组以及长度不超过 `N` 的 `values` 数组，空间复杂度为 $O(N+C)$。
