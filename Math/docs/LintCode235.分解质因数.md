# [LintCode235.分解质因数](https://www.jiuzhang.com/solution/prime-factorization/)
## 题目描述
将一个整数分解为若干质因数之乘积。

注意:

你需要从小到大排列质因子。
### 样例
```
输入：10
输出：[2, 5]

输入：660
输出：[2, 2, 3, 5, 11]
```
## 题解
如何确定存下的因数一定是质数呢？因为一个非质数一定可以分解成几个质数的乘积，所以在判断到不是质数的因数之前这个因数就已经被分解了。
```java
class Solution {
    /**
     * @param num an integer
     * @return an integer array
     */
    public List<Integer> primeFactorization(int num) {
        List<Integer> factors = new ArrayList<Integer>();
        for (int i = 2; i * i <= num; i++) {
            while (num % i == 0) {
                num = num / i;
                factors.add(i);
            }
        }
        //若最后剩余数不为1，则为最后一个质因数
        if (num != 1) {
            factors.add(num);
        }   
        return factors;
    }
}
```
### 复杂度分析
- 时间复杂度:$O(\sqrt{n})$，
- 空间复杂度:$O(1)$，一个`[1,1e9]`范围内的数，质因数个数不会超过32个，因此空间复杂度为常数复杂度。