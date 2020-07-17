# [剑指Offer26.树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)
## 题目描述
输入两棵二叉树`A`和`B`，判断`B`是不是`A`的子结构。(约定空树不是任意一个树的子结构)

`0 <= 节点个数 <= 10000`

`B`是`A`的子结构， 即 `A`中有出现和`B`相同的结构和节点值。

例如

给定的树 `A`:
```
     3
    / \
   4   5
  / \
 1   2
```
 给定的树 `B`：
```
   4 
  /
 1
```
返回 `true`，因为 `B` 与 `A` 的一个子树拥有相同的结构和节点值。
### 示例
```
输入：A = [1,2,3], B = [3,1]
输出：false
```
```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```
## 题解
```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        return (A != null && B != null) && (recur(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B));
    }
    boolean recur(TreeNode A, TreeNode B) {
        if(B == null) return true;
        if(A == null || A.val != B.val) return false;
        return recur(A.left, B.left) && recur(A.right, B.right);
    }
}
```
### 复杂度分析
- 时间复杂度 $O(MN)$： 其中 `M`,`N`分别为树 `A` 和 树 `B` 的节点数量；先序遍历树 `A` 占用 $O(M)$ ，每次调用 `recur(A, B)` 判断占用 $O(N)$。
- 空间复杂度 $O(M)$： 当树 `A` 和树 `B` 都退化为链表时，递归调用深度最大。当 $M≤N$时，遍历树 `A` 与递归判断的总递归深度为 `M` ；当 $M>N$ 时，最差情况为遍历至树 `A` 叶子节点，此时总递归深度为 `M`。


