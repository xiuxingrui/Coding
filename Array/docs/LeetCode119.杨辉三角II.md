# [LeetCode119.杨辉三角II](https://leetcode-cn.com/problems/pascals-triangle-ii/)
## 题目描述
给定一个非负索引 `k`，其中 `k ≤ 33`，返回杨辉三角的第 `k` 行。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201005185637.png)

在杨辉三角中，每个数是它左上方和右上方的数的和。

进阶：你可以优化你的算法到 $O(k)$ 空间复杂度吗？
### 示例
```
输入: 3
输出: [1,3,3,1]
```
## 题解
我们只需要一层一层的求。但是不需要把每一层的结果都保存起来，只需要保存上一层的结果，就可以求出当前层的结果了。

```java
public List<Integer> getRow(int rowIndex) {
    List<Integer> pre = new ArrayList<>();
    List<Integer> cur = new ArrayList<>();
    for (int i = 0; i <= rowIndex; i++) {
        cur = new ArrayList<>();
        for (int j = 0; j <= i; j++) {
            if (j == 0 || j == i) {
                cur.add(1);
            } else {
                cur.add(pre.get(j - 1) + pre.get(j));
            } 
        }
        pre = cur;
    }
    return cur;
}
```
其实我们可以优化一下，我们可以把 `pre` 的 `List` 省去。

这样的话，`cur`每次不去新建 `List`，而是把`cur`当作`pre`。

又因为更新当前`j`的时候，就把之前j的信息覆盖掉了。而更新 `j + 1` 的时候又需要之前`j`的信息，所以在更新前，我们需要一个变量把之前`j`的信息保存起来。
```java
public List<Integer> getRow(int rowIndex) {
    int pre = 1;
    List<Integer> cur = new ArrayList<>();
    cur.add(1);
    for (int i = 1; i <= rowIndex; i++) {
        for (int j = 1; j < i; j++) {
            int temp = cur.get(j);
            cur.set(j, pre + cur.get(j));
            pre = temp;
        }
        cur.add(1);
    }
    return cur;
}
```

区别在于我们用了 `set` 函数来修改值，由于当前层比上一层多一个元素，所以对于最后一层的元素如果用 `set` 方法的话会造成越界。此外，每层的第一个元素始终为1。基于这两点，我们把之前`j == 0 || j == i`的情况移到了`for`循环外进行处理。

除了上边优化的思路，还有一种想法，那就是倒着进行，这样就不会存在覆盖的情况了。

因为更新完`j`的信息后，虽然把`j`之前的信息覆盖掉了。但是下一次我们更新的是`j - 1`，需要的是`j - 1`和`j - 2` 的信息，`j`信息覆盖就不会造成影响了。

自己的解法：
```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        int[] ans=new int[rowIndex+1];
        ans[0]=1;
        for(int i=1;i<=rowIndex;i++){
            for(int j=i;j>=1;j--){
                ans[j]+=ans[j-1];
            }
        }
        List<Integer> res=new ArrayList<>();
        for(int num:ans){
            res.add(num);
        }
        return res;
    }
}
```

