# [LeetCode437.路径总和III](https://leetcode-cn.com/problems/path-sum-iii/)
## 题目描述
给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

### 示例
```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```
## 题解
### 解法一
```java
// 九章算法
class Solution {
//pathSum: 返回以root为根的树中，所有路径和为sum的路径个数（以root开头，不以root开头）
    public int pathSum(TreeNode root, int sum) {
        if (root == null) {
            return 0;
        }
        //由三部分组成：不以root开头的路径（1. 递归左子树结果 2.递归右子树结果 ）
        //3. 必须以root开头的路径                        
        return pathSum(root.left, sum) + pathSum(root.right, sum) + findPath(root, sum);
    }

    //统计必须从root出发，路径和为sum的所有路径个数
    public int findPath(TreeNode root, int sum) {
        if (root == null) {
            return 0;
        }
        int res = 0;//由三部分组成
        if (sum == root.val) { //已经找到了以root结尾的路径。累加进res
            res += 1;
        }
        //因为后序路径可能存在类似于加n减n的情况，所以需要继续递归寻找左右子树
        res += findPath(root.left, sum - root.val);
        res += findPath(root.right, sum - root.val);
        return res;
    }
}
```