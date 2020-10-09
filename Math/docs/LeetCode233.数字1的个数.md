# [LeetCode233.数字1的个数](https://leetcode-cn.com/problems/number-of-digit-one/)
## 题目描述
给定一个整数 `n`，计算所有小于等于 `n` 的非负整数中数字 1 出现的个数。

### 示例
```
输入: 13
输出: 6 
解释: 数字 1 出现在以下数字中: 1, 10, 11, 12, 13 。
```
## 题解
### 解法一
借鉴密码锁的思想：

将 `1 ~ n` 的个位、十位、百位、...的 1 出现次数相加，即为 1 出现的总次数。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009153043.png)

某位中 1 出现次数的计算方法：

根据当前位 `cur` 值的不同，分为以下三种情况：

1. 当 `cur=0`时： 此位 1 的出现次数只由高位 `high` 决定，计算公式为：`high×digit`

如下图所示，以 `n=2304` 为例，求 `digit=10` （即十位）的 1 出现次数。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009153219.png)

2. 当 `cur=1` 时： 此位 1 的出现次数由高位 `high` 和低位 `low` 决定，计算公式为：`high×digit+low+1`

如下图所示，以 `n=2314` 为例，求 `digit=10` （即十位）的 1 出现次数。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009153419.png)
3. 当 `cur=2,3,⋯,9`时： 此位 1 的出现次数只由高位 `high` 决定，计算公式为：`(high+1)×digit`

如下图所示，以 `n=2324` 为例，求 `digit=10` （即十位）的 1 出现次数。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009153619.png)

变量递推公式：
设计按照 “个位、十位、...” 的顺序计算，则 `high/cur/low/digit`应初始化为：

```java
high = n / 10
cur = n % 10
low = 0
digit = 1 # 个位
```
因此，从个位到最高位的变量递推公式为：
```java
while (high != 0 || cur != 0){ # 当 high 和 cur 同时为 0 时，说明已经越过最高位，因此跳出
   low += cur * digit # 将 cur 加入 low ，组成下轮 low
   cur = high % 10 # 下轮 cur 是本轮 high 的最低位
   high /= 10 # 将本轮 high 最低位删除，得到下轮 high
   digit *= 10 # 位因子每轮 × 10
}
```
```java
class Solution {
    public int countDigitOne(int n) {
        if(n<=0){
            return 0;
        }
        int digit = 1, res = 0;
        int high = n / 10, cur = n % 10, low = 0;
        while(high != 0 || cur != 0) {
            if(cur == 0){
                res += high * digit;
            }
            else if(cur == 1){
                res += high * digit + low + 1;
            }
            else{
                res += (high + 1) * digit;
            }
            low += cur * digit;
            cur = high % 10;
            high /= 10;
            digit *= 10;
        }
        return res;
    }
}
```
#### 复杂度分析
- 时间复杂度 $O(logn)$： 循环内的计算操作使用 $O(1)$时间；循环次数为数字 `n` 的位数，因此循环使用 $O(logn)$时间。
- 空间复杂度 $O(1)$： 几个变量使用常数大小的额外空间。

### 解法二
暴力法：超时
```java
class Solution {
    public int countDigitOne(int n) {
        int cnt=0;
        for(int i=1;i<=n;i++){
            int count=0;
            int num=i;
            while(num!=0){
                if(num%10==1){
                    count++;
                }
                num/=10;
            }
            cnt+=count;
        }
        return cnt;
    }
}
```
#### 复杂度分析
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201009152108.png)

