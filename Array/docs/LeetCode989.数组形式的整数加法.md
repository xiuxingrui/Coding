# [LeetCode989.数组形式的整数加法](https://leetcode-cn.com/problems/add-to-array-form-of-integer/)
## 题目描述
对于非负整数 `X` 而言，`X` 的数组形式是每位数字按从左到右的顺序形成的数组。例如，如果 `X = 1231`，那么其数组形式为 `[1,2,3,1]`。

给定非负整数 `X` 的数组形式 `A`，返回整数 `X+K` 的数组形式。

### 示例
```
输入：A = [1,2,0,0], K = 34
输出：[1,2,3,4]
解释：1200 + 34 = 1234
```
```
输入：A = [2,7,4], K = 181
输出：[4,5,5]
解释：274 + 181 = 455
```
```
输入：A = [2,1,5], K = 806
输出：[1,0,2,1]
解释：215 + 806 = 1021
```
## 题解
```java
class Solution {
    public List<Integer> addToArrayForm(int[] A, int K) {
        Deque<Integer> stack=new LinkedList<>();
        List<Integer> list=new ArrayList<>();
        while(K!=0){
            int tmp=K%10;
            stack.push(tmp);
            K/=10;
        }
        int len=stack.size();
        int[] B=new int[len];
        int cnt=0;
        while(!stack.isEmpty()){
            B[cnt++]=stack.pop();
        }
        int flag=0,i=A.length-1,j=len-1;
        while(i>=0||j>=0||flag==1){
            int val1=i>=0?A[i]:0;
            int val2=j>=0?B[j]:0;
            int sum=flag+val1+val2;
            flag=sum/10;
            stack.push(sum%10);
            i--;
            j--;
        }
        while(!stack.isEmpty()){
            list.add(stack.pop());
        }
        return list;
    }
}
```
优化:
```java
class Solution {
    public List<Integer> addToArrayForm(int[] A, int K) {
        Deque<Integer> stack=new LinkedList<>();
        LinkedList<Integer> list=new LinkedList<>();
        while(K!=0){
            int tmp=K%10;
            stack.push(tmp);
            K/=10;
        }
        int len=stack.size();
        int[] B=new int[len];
        int cnt=0;
        while(!stack.isEmpty()){
            B[cnt++]=stack.pop();
        }
        int flag=0,i=A.length-1,j=len-1;
        while(i>=0||j>=0||flag==1){
            int val1=i>=0?A[i]:0;
            int val2=j>=0?B[j]:0;
            int sum=flag+val1+val2;
            flag=sum/10;
            list.addFirst(sum%10);
            i--;
            j--;
        }
        return list;
    }
}
```