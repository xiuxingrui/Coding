# [剑指Offer10-III.矩形覆盖](https://www.nowcoder.com/practice/72a5a919508a4251859fb2cfb987a0e6?tpId=13&&tqId=11163&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
## 题目描述
我们可以用`2*1`的小矩形横着或者竖着去覆盖更大的矩形。请问用`n`个`2*1`的小矩形无重叠地覆盖一个`2*n`的大矩形，总共有多少种方法？

比如n=3时，2*3的矩形块有3种覆盖方法：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201119205507.png)

## 题解
自己的想法：

覆盖的策略可以认为是只能从左边开始覆盖，可以避免重复。

我们先把 `2*n` 的覆盖方法记为 `f(n)`。用第一个 2×1 的小矩形去覆盖大矩形的最左边时有两种选择：竖着放或者横着放。当竖着放的时候，右边还剩下 `2×(n-1)` 的区域，这种情形下的覆盖方法记为 `f(n-1)`。接下来考虑横着放的情况。当 2x1 的小矩形横着放在左上角的时候，左下角必须和横着放一个 2x1 的小矩形，而在右边还剩下 `2×(n-2)` 的区域，这种情形下的覆盖方法记为`f(n-2)`，因此 `f(n) =f(n-1)+f(n-2)`。此时我们可以看出，这仍然是斐波那契数列。
```java
public class Solution {
    public int RectCover(int target) {
        if(target==0||target==1||target==2){
            return target;
        }
        int[] dp=new int[target+1];
        dp[1]=1;
        dp[2]=2;
        for(int i=3;i<=target;i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[target];
    }
}
```
```java
public class Solution {
    public int RectCover(int target) {
        if(target==0){
            return 0;
        }
        int a=1,b=1;
        for(int i=0;i<target-1;i++){
            int c=a+b;
            a=b;
            b=c;
        }
        return b;
    }
}
```
```java
public class Solution {
    public int RectCover(int target) {
        if(target==0){
            return 0;
        }
        if(target==1){
            return 1;
        }
        if(target==2){
            return 2;
        }
        return RectCover(target-1)+RectCover(target-2);
    }
}
```