# [剑指Offer46.把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)
## 题目描述
给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 `“a”` ，1 翻译成 `“b”`，……，11 翻译成 `“l”`，……，25 翻译成 `“z”`。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

- `0 <= num < 2^31`

### 示例
```
输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```
## 题解
### 解法一
首先我们来通过一个例子理解一下这里「翻译」的过程：我们来尝试翻译`「1402」`。

分成两种情况：

- 首先我们可以把每一位单独翻译，即 `[1,4,0,2]`，翻译的结果是 `beac`
- 然后我们考虑组合某些连续的两位：
  - `[14,0,2]`，翻译的结果是 `oac`。
  - `[1,40,2]`，这种情况是不合法的，因为 40 不能翻译成任何字母。
  - `[1,4,02]`，这种情况也是不合法的，含有前导零的两位数不在题目规定的翻译规则中，那么 `[14,02]` 显然也是不合法的。
那么我们可以归纳出翻译的规则，字符串的第 `i` 位置：
- 可以单独作为一位来翻译
- 如果第 `i−1` 位和第 `i` 位组成的数字在 10 到 25 之间，可以把这两位连起来翻译

到这里，我们发现它和「198. 打家劫舍」非常相似。我们可以用 `f(i)` 表示以第 `i` 位结尾的前缀串翻译的方案数，考虑第 `i` 位单独翻译和与前一位连接起来再翻译对 `f(i)` 的贡献。单独翻译对 `f(i)` 的贡献为 `f(i−1)`；如果第 `i−1` 位存在，并且第 `i−1` 位和第 `i` 位形成的数字 `x` 满足 `10≤x≤25`，那么就可以把第 `i−1` 位和第 `i` 位连起来一起翻译，对 `f(i)` 的贡献为 `f(i−2)`，否则为 0。我们可以列出这样的动态规划转移方程：

`f(i)=f(i−1)+f(i−2)[i−2≥0,10≤x≤25]`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200925180059.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200925181010.png)

```java
class Solution {
    public int translateNum(int num) {
        String s=String.valueOf(num);
        int len=s.length();
        if(len<2){
            return 1;
        }
        int[] dp=new int[len];
        dp[0]=1;
        for(int i=1;i<len;i++){
            dp[i]=dp[i-1];
            int currentNum=(s.charAt(i-1)-'0')*10+s.charAt(i)-'0';
            if(currentNum>9&&currentNum<26){
                if(i<2){
                    dp[i]++;
                }else{
                    dp[i]+=dp[i-2];
                }
            }
        }
        return dp[len-1];
    }
}
```
优化定义
```java
class Solution {
    public int translateNum(int num) {
        String s=String.valueOf(num);
        int len=s.length();
        if(len<2){
            return 1;
        }
        int[] dp=new int[len+1];
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<len+1;i++){
            dp[i]=dp[i-1];
            int currentNum=(s.charAt(i-2)-'0')*10+s.charAt(i-1)-'0';
            if(currentNum>9&&currentNum<26){
                dp[i]+=dp[i-2];
            }
        }
        return dp[len];
    }
}
```
#### 复杂度分析
记 `num=n`。
- 时间复杂度：循环的次数是 `n` 的位数，故渐进时间复杂度为 $O(logn)$。

### 解法
```java
class Solution {
    public int translateNum(int num) {
        if(num==0){
            return 1;
        }
        int temp=num;
        int cnt=0;
        while(temp!=0){
            cnt++;
            temp/=10;
        }
        int[] nums=new int[cnt];
        temp=num;
        int idx=cnt-1;
        while(temp!=0){
            nums[idx--]=temp%10;
            temp/=10;
        }
        return helper(nums,0,cnt-1);
    }
    public int helper(int[] nums,int i,int j){
        if(i>j){
            return 0;
        }
        if(i==j){
            return 1;
        }
        if(j==i+1){
            int a=nums[i];
            int b=nums[i+1];
            int c=a*10+b;
            if(c<10){
                return 1;
            }
            if(c<26){
                return 2;
            }else{
                return 1;
            }
        }
        int ans=helper(nums,i+1,j);
        int a=nums[i];
        int b=nums[i+1];
        int c=a*10+b;
        if(9<c&&c<26){
            ans+=helper(nums,i+2,j);
        }
        return ans;
    }
}
```