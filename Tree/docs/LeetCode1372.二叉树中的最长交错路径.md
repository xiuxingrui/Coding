# [LeetCode1372.二叉树中的最长交错路径](https://leetcode-cn.com/problems/longest-zigzag-path-in-a-binary-tree/)
## 题目描述
给你一棵以 `root` 为根的二叉树，二叉树中的交错路径定义如下：

- 选择二叉树中 任意 节点和一个方向（左或者右）。
- 如果前进方向为右，那么移动到当前节点的的右子节点，否则移动到它的左子节点。
- 改变前进方向：左变右或者右变左。
- 重复第二步和第三步，直到你在树中无法继续移动。

交错路径的长度定义为：访问过的节点数目 - 1（单个节点的路径长度为 0 ）。

请你返回给定树中最长 交错路径 的长度。

- 每棵树最多有 50000 个节点。
- 每个节点的值在 `[1, 100]` 之间。

### 示例
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200908163549.png)

```
输入：root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1]
输出：3
解释：蓝色节点为树中最长交错路径（右 -> 左 -> 右）。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200908163609.png)

```
输入：root = [1,1,1,null,1,null,null,1,1,null,1]
输出：4
解释：蓝色节点为树中最长交错路径（左 -> 右 -> 左 -> 右）。
```
```
输入：root = [1]
输出：0
```
## 题解
### 解法一：树形DP
- `res[0]`表示当前节点下一步向左走带来的最大收益，`res[1]`表示当前节点下一步向右走带来的最大收益
- `res[0]=1+left[1]` 当前节点下一步向左走带来的最大收益等于左子节点向右走的最大收益+1
- `res[1]=1+right[0]` 当前节点下一步向右走带来的最大收益等于右子节点向左走的最大收益+1

维护一个全局变量`maxPath`，每次遍历某一节点时，更新它
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
  public int maxPath = 0;

  public int longestZigZag(TreeNode root) {
    dfs(root);
    return maxPath;
  }

  private int[] dfs(TreeNode root) {
    int[] res = new int[2];
    if (root == null) {
      res[0] = -1;
      res[1] = -1;
     return res;
    }
    int[] left = dfs(root.left);
    int[] right = dfs(root.right);
    res[0] = 1 + left[1];
    res[1] = 1 + right[0];
    maxPath = Math.max(maxPath, Math.max(res[0], res[1]));
    return res;
  }
}
```

### 解法二：DFS
自己的写法
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int ans=Integer.MIN_VALUE;
    public int longestZigZag(TreeNode root) {
        if(root==null){
            return 0;
        }
        dfs(root,true);
        dfs(root,false);
        return ans-1;
    }
    public int dfs(TreeNode root,boolean flag){
        if(root==null){
            return 0;
        }
        int rightLen=dfs(root.right,false);
        int leftLen=dfs(root.left,true);
        ans=Math.max(ans,Math.max(rightLen,leftLen)+1);
        if(flag==true){
            return rightLen+1;
        }else{
            return leftLen+1;
        }
    }
}
```
#### 复杂度分析 
- 时间复杂度：在 `DFS` 的过程中，这棵树中的所有节点都会被访问一次，所以如果节点总数是 `n` 的话，那么渐进时间复杂度就是 $O(n)$。
- 空间复杂度：这里用到的辅助空间是 `maxAns` 变量，所以渐进空间复杂度是 $O(1)$。


