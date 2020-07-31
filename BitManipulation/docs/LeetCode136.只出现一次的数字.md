# [LeetCode136.只出现一次的数字](https://leetcode-cn.com/problems/single-number/)
## 题目描述
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

### 示例
```
输入: [2,2,1]
输出: 1
```
```
输入: [4,1,2,1,2]
输出: 4
```
## 题解
### 解法一
对于这道题，可使用异或运算$⊕$。异或运算有以下三个性质。
1. 任何数和 0 做异或运算，结果仍然是原来的数，即 $a⊕0=a$。
2. 任何数和其自身做异或运算，结果是 0，即 $a⊕a=0$。
3. 异或运算满足交换律和结合律，即 $a⊕b⊕a=b⊕a⊕a=b⊕(a⊕a)=b⊕0=b$。

假设数组中有 `2m+1` 个数，其中有`m`个数各出现两次，一个数出现一次。令$a_{1}, a_{2}, \ldots, a_{m}$为出现两次的`m`个数，$a_{m+1}$为出现一次的数。根据性质3，数组中的全部元素的异或运算结果总是可以写成如下形式：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200730192513.png)

根据性质 2 和性质 1，上式可化简和计算得到如下结果：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200730192531.png)

因此，数组中的全部元素的异或运算结果即为数组中只出现一次的数字。

```java
class Solution {
    public int singleNumber(int[] nums) {
        int single = 0;
        for (int num : nums) {
            single ^= num;
        }
        return single;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是数组长度。只需要对数组遍历一次。
- 空间复杂度：$O(1)$。

### 解法二
```java
class Solution {
  public int singleNumber(int[] nums) {
    HashMap<Integer, Integer> hashmap = new HashMap<>();
    for (int num : nums)
      hashmap.put(num, hashmap.getOrDefault(num, 0) + 1);

    for (int k : hashmap.keySet())
      if (hashmap.get(k) == 1) return k;
    return -1;
  }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，其中 `n` 是数组长度。只需要对数组遍历一次。
- 空间复杂度：$O(n)$。
### 解法三
```java
class Solution {
  public int singleNumber(int[] nums) {
    Set<Long> set = new HashSet<>();
    long sumSet = 0, sumArray = 0;
    for(int n : nums) {
      sumArray += n;
      set.add((long)n);
    }
    for(Long s : set) sumSet += s;
    return (int)(2* sumSet - sumArray);
  }
}
```
#### 复杂度分析

- 时间复杂度：$O(N)$，遍历输入数组。
- 空间复杂度：$O(N)$，存储 $N/2$ 个元素的集合。


