# [LeetCode118.杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)
## 题目描述
给定一个非负整数 `numRows`，生成杨辉三角的前 `numRows` 行。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201005185637.png)

在杨辉三角中，每个数是它左上方和右上方的数的和。

### 示例
```
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
## 题解
```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans=new ArrayList<>();
        for(int i=0;i<numRows;i++){
            if(i==0){
                List<Integer> temp=new ArrayList<>();
                temp.add(1);
                ans.add(temp);
            }else{
                List<Integer> temp=new ArrayList<>();
                temp.add(1);
                for(int j=0;j<i-1;j++){
                    temp.add(ans.get(i-1).get(j)+ans.get(i-1).get(j+1));
                }
                temp.add(1);
                ans.add(temp);
            }
        }
        return ans;
    }
}
```

