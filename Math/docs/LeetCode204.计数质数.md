# [LeetCode204.计数质数](https://leetcode-cn.com/problems/count-primes/)
## 题目描述
统计所有小于非负整数 `n` 的质数的数量。
### 示例
```java
输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```
## 题解
```java
class Solution {
    public int countPrimes(int n) {
        boolean[] noPrime=new boolean[n];
        int count=0;
        for(int i=2;i<n;++i){
            if(noPrime[i]==true){
                continue;
            }
            int rate=2;
            while(i*rate<n){
                noPrime[i*rate]=true;
                ++rate;
            }
        }
        for(int i=2;i<n;++i){
            if(noPrime[i]==false){
                count++;
            }
        }
        return count;
    }
}
```
别人的原始方法:
```java
class Solution {
    int countPrimes(int n) {
    boolean[] isPrim = new boolean[n];
    // 将数组都初始化为 true
    Arrays.fill(isPrim, true);

    for (int i = 2; i < n; i++) 
        if (isPrim[i]) 
            // i 的倍数不可能是素数了
            for (int j = 2 * i; j < n; j += i) 
                    isPrim[j] = false;
    
    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrim[i]) count++;
    
    return count;
    }
}
```
优化：

判断一个数是否是素数的 `isPrime` 函数，由于因子的对称性，其中的 `for` 循环只需要遍历 `[2,sqrt(n)]` 就够了。这里外层的 `for` 循环也只需要遍历到 `sqrt(n)`:
```java
for (int i = 2; i * i < n; i++) 
    if (isPrim[i]) 
        ...
```
内层的 `for` 循环也可以优化。我们之前的做法是：
```java
for (int j = 2 * i; j < n; j += i) 
    isPrim[j] = false;
```
这样可以把 `i` 的整数倍都标记为 `false`，但是仍然存在计算冗余。

比如 `n = 25`，`i = 4` 时算法会标记 4 × 2 = 8，4 × 3 = 12 等等数字，但是这两个数字已经被 `i = 2` 和 `i = 3` 的 2 × 4 和 3 × 4 标记了。

我们可以稍微优化一下，让 `j` 从 `i` 的平方开始遍历，而不是从 `2 * i` 开始：
```java
for (int j = i * i; j < n; j += i) 
    isPrim[j] = false;
```
优化后完整代码:
```java
class Solution {
    int countPrimes(int n) {
    boolean[] isPrim = new boolean[n];
    Arrays.fill(isPrim, true);
    for (int i = 2; i * i < n; i++) 
        if (isPrim[i]) 
            for (int j = i * i; j < n; j += i) 
                isPrim[j] = false;
    
    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrim[i]) count++;
    
    return count;
    }
}
```
暴力法:
```java
class Solution {
    public int countPrimes(int n) {
        int ans = 0;
        for (int i = 2; i < n; ++i) {
            ans += isPrime(i) ? 1 : 0;
        }
        return ans;
    }

    public boolean isPrime(int x) {
        for (int i = 2; i * i <= x; ++i) {
            if (x % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```




