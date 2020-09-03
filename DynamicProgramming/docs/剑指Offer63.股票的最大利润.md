# [剑指Offer63.股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)
## 题目描述
假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

### 示例
```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```
```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```
## 题解
状态定义： 设动态规划列表 `dp` ，`dp[i]` 代表以 `prices[i]` 为结尾的子数组的最大利润（以下简称为 前 `i` 日的最大利润 ）。

转移方程： 由于题目限定 “买卖该股票一次” ，因此前 `i` 日最大利润 `dp[i]` 等于前 `i−1`日最大利润 `dp[i−1]`和第 `i` 日卖出的最大利润中的最大值。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200903182831.png)

初始状态： `dp[0]=0` ，即首日利润为 0 ；

返回值： `dp[n−1]`，其中`n`为 `dp` 列表长度。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200903182912.png)

时间复杂度降低： 前 `i` 日的最低价格 `min(prices[0:i])`时间复杂度为 $O(i)$。而在遍历 `prices` 时，可以借助一个变量（记为成本 `cost` ）每日更新最低价格。优化后的转移方程为：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200903183014.png)

空间复杂度降低： 由于 `dp[i]` 只与 `dp[i−1]`, `prices[i]`, `cost` 相关，因此可使用一个变量（记为利润 `profit`）代替 `dp`列表。优化后的转移方程为：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200903183057.png)



```java
public class Solution {
    public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice)
                minprice = prices[i];
            else if (prices[i] - minprice > maxprofit)
                maxprofit = prices[i] - minprice;
        }
        return maxprofit;
    }
}
```
写法二:
```java
class Solution {
    public int maxProfit(int[] prices) {
        int cost = Integer.MAX_VALUE, profit = 0;
        for(int price : prices) {
            cost = Math.min(cost, price);
            profit = Math.max(profit, price - cost);
        }
        return profit;
    }
}
```
自己写法：
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null||prices.length==0||prices.length==1){
            return 0;
        }
        int[] minValue=new int[prices.length];
        minValue[0]=prices[0];
        int ans=0;
        for(int i=1;i<prices.length;i++){
            minValue[i]=Math.min(minValue[i-1],prices[i]);
            ans=Math.max(ans,prices[i]-minValue[i]);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(n)$，只需要遍历一次。
- 空间复杂度：$O(1)$，只使用了常数个变量。