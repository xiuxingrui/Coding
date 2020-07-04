# LeetCode52.N皇后II(https://leetcode-cn.com/problems/n-queens-ii/)
## 题目描述
`n` 皇后问题研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。皇后可以攻击同一行、同一列、左上左下右上右下四个方向的任意单位。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200704211515.png)

上图为 8 皇后问题的一种解法。

给定一个整数 `n`，返回 `n` 皇后不同的解决方案的数量。
### 示例
```
输入: 4
输出: 2
解释: 4 皇后问题存在如下两个不同的解法。
[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
```
## 题解
```java
class Solution {
    public int count=0;
    public int totalNQueens(int n){
        List<List<String>> res=new ArrayList<>();
        ArrayList<Integer> path=new ArrayList<>();
        int[] nums=new int[n];
        for(int i=0;i<n;++i){
            nums[i]=i;
        }
        boolean[] used=new boolean[n];
        helper(res,path,nums,used,0,n);
        return count;
    }
    public void helper(List<List<String>> res,ArrayList<Integer> path,int[] nums,boolean[] used,int p,int n){
        if(path.size()==n){
            ++count;
        }
        for(int k=0;k<n;++k){
            if(used[k]) continue;
            boolean is=true;
            for(int l=0;l<p;++l){
                if(Math.abs(p-l)==Math.abs(nums[k]-path.get(l))){
                    is=false;
                    break;
                }
            }
            if(is==true){
                used[k]=true;
                path.add(nums[k]);
                helper(res,path,nums,used,p+1,n);
                path.remove(p);
                used[k]=false;
            }
        }
    }
}
```
#### 复杂度分析
- 时间复杂度:$O(n!)$
- 空间复杂度:$O(n)$