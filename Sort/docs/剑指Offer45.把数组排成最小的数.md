# [剑指Offer45.把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)
## 题目描述
输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

- `0 < nums.length <= 100`

- 输出结果可能非常大，所以你需要返回一个字符串而不是整数
- 拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0
### 示例
```
输入: [10,2]
输出: "102"
```
```
输入: [3,30,34,5,9]
输出: "3033459"
```
## 题解
此题求拼接起来的 “最小数字” ，本质上是一个排序问题。

排序判断规则： 设 `nums` 任意两数字的字符串格式 `x` 和 `y` ，则
- 若拼接字符串 `x+y>y+x`，则 `x>y`；
- 反之，若 `x+y<y+x`，则 `x<y`；

根据以上规则，套用任何排序方法对 `nums` 执行排序即可。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924224628.png)

算法流程：
- 初始化： 字符串列表 `strs` ，保存各数字的字符串格式；
- 列表排序： 应用以上 “排序判断规则” ，对 `strs` 执行排序；
- 返回值： 拼接 `strs` 中的所有字符串，并返回。

快速排序：

需修改快速排序函数中的排序判断规则。字符串大小（字典序）对比的实现方法：

`Java` 中使用 `A.compareTo(B)`。

```java
class Solution {
    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++)
            strs[i] = String.valueOf(nums[i]);
        fastSort(strs, 0, strs.length - 1);
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
    }
    void fastSort(String[] strs, int l, int r) {
        if(l >= r) return;
        int i = l, j = r;
        String tmp = strs[i];
        while(i < j) {
            while((strs[j] + strs[l]).compareTo(strs[l] + strs[j]) > 0 && i < j) j--;
            while((strs[i] + strs[l]).compareTo(strs[l] + strs[i]) <= 0 && i < j) i++;
            tmp = strs[i];
            strs[i] = strs[j];
            strs[j] = tmp;
        }
        strs[i] = strs[l];
        strs[l] = tmp;
        fastSort(strs, l, i - 1);
        fastSort(strs, i + 1, r);
    }
}
```

内置函数：

`Java` 定义为 `(x, y) -> (x + y).compareTo(y + x)` 。

```java
class Solution {
    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++) 
            strs[i] = String.valueOf(nums[i]);
        Arrays.sort(strs, (x, y) -> (x + y).compareTo(y + x));
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
    }
}
```
思路描述：首先我们要明白就是

无论这些数字怎么取排列，形成的数字的位数是不变的，那么就是高位的数字肯定是越小越好。

我们先考虑一下怎么排列两个数字，比如 1 和 20，高位越小越好，放 1，组合成 120

我们再看一下三个数的情况，比如 36、38 和 5，首先肯定先放 36，剩下 38 和 5，然后对这两个数进行排列 385，所以最后的结果为 36385。

由上面的两个例子我们其实就可以知道，放数字的顺序肯定是先放第一位(最左边一位)最小的元素，如果第一位相等，比较第二位....，以此类推。

我们再思考一下，36 < 38 > 5 ，但是 "36" < "38" < "5"，

也就是我们如果把所有数字转换成字符串再排列，刚好就是我们希望的情况。

注意：我们这里说的排列大小比较和字符串大小有点区别，比如 3 和 30，明显 30 排在前面比较好，所以我们要重构比较，我们组合 `s1` 和 `s2` ，如果 `s1 + s2 > s2 + s1`，那么 `s1 > s2`

至此，我们已经分析出来了。

剑指Offer上的解释：
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924225756.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924225814.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200924225825.png)

### 复杂度分析：
- 时间复杂度 $O(NlogN)$： `N` 为最终返回值的字符数量（ `strs列表的长度 ≤N`）；使用快排或内置函数的平均时间复杂度为 $O(NlogN)$，最差为 $O(N^2)$。
- 空间复杂度 $O(N)$： 字符串列表 `strs` 占用线性大小的额外空间。

