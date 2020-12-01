# [LeetCode236.二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
## 题目描述
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先的定义为：“对于有根树 `T` 的两个结点 `p`、`q`，最近公共祖先表示为一个结点 `x`，满足 `x` 是 `p`、`q` 的祖先且 `x` 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  `root = [3,5,1,6,2,0,8,null,null,7,4]`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200714175105.png)

### 示例
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```
### 注意
- 所有节点的值都是唯一的。
- `p`、`q` 为不同节点且均存在于给定的二叉树中。

## 题解
### 解法一:递归
版本1：

最近公共祖先的定义： 设节点 `root` 为节点 `p`,`q`的某公共祖先，若其左子节点 `root.left`和右子节点 `root.right`都不是 `p`,`q` 的公共祖先，则称 `root` 是 “最近的公共祖先” 。

根据以上定义，若 `root` 是 `p`,`q` 的 最近公共祖先 ，则只可能为以下情况之一：

1. `p` 和 `q` 在 `root` 的子树中，且分列 `root` 的 异侧（即分别在左、右子树中）；
2. `p=root` ，且 `q` 在 `root` 的左或右子树中；
3. `q=root`，且 `p` 在 `root` 的左或右子树中；

考虑通过递归对二叉树进行后序遍历，当遇到节点 `p` 或 `q` 时返回。从底至顶回溯，当节点 `p`,`q`在节点 `root` 的异侧时，节点 `root` 即为最近公共祖先，则向上返回 `root` 。

递归解析：

1. 终止条件：

   1. 当越过叶节点，则直接返回 `null` ；
   2. 当 `root` 等于 `p`,`q`，则直接返回 `root` ；
2. 递推工作：
   1. 开启递归左子节点，返回值记为 `left`；
   2. 开启递归右子节点，返回值记为 `right`；
3. 返回值： 根据 `left` 和 `right` ，可展开为四种情况；
   1. 当 `left`和 `right` 同时为空 ：说明 `root` 的左 / 右子树中都不包含 `p`,`q`，返回 `null` ；
   2. 当 `left` 和 `right` 同时不为空 ：说明 `p`,`q` 分列在 `root` 的 异侧 （分别在 左 / 右子树），因此 `root`为最近公共祖先，返回 `root`；
   3. 当 `left` 为空 ，`right` 不为空 ：`p`,`q`都不在 `root` 的左子树中，直接返回 `right` 。具体可分为两种情况：
      1. `p`,`q` 其中一个在 `root` 的 右子树 中，此时 `right` 指向 `p`（假设为 `p` ）；
      2. `p`,`q` 两节点都在 `root` 的 右子树 中，此时的 `right` 指向 最近公共祖先节点 ；
   4. 当 `left` 不为空 ， `right` 为空 ：与情况 3. 同理；

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null||root==p||root==q){
            return root;
        }
        TreeNode leftNode=lowestCommonAncestor(root.left,p,q);
        TreeNode rightNode=lowestCommonAncestor(root.right,p,q);

        if(leftNode == null && rightNode == null) return null; // 1.
        if(leftNode == null) return rightNode; // 3.
        if(rightNode == null) return leftNode; // 4.
        return root; // 2. if(left != null and right != null)
    }
}
```
简化：
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;
        TreeNode leftnode = lowestCommonAncestor(root.left, p, q);
        TreeNode rightNode = lowestCommonAncestor(root.right, p, q);
        if(leftNode == null) return rightNode;
        if(rightNode == null) return leftNode;
        return root;
    }
}
```
版本2：

定义$f_{x}$表示 `x` 节点的子树中是否包含 `p` 节点或 `q` 节点，如果包含为 `true`，否则为 `false`。那么符合条件的最近公共祖先 `x` 一定满足如下条件：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201201142640.png)


其中 `lson` 和 `rson` 分别代表 `x` 节点的左孩子和右孩子。

你可能会疑惑这样找出来的公共祖先深度是否是最大的。其实是最大的，因为我们是自底向上从叶子节点开始更新的，所以在所有满足条件的公共祖先中一定是深度最大的祖先先被访问到，且由于 $f_{x}$ 本身的定义很巧妙，在找到最近公共祖先 `x` 以后，$f_{x}$按定义被设置为 `true` ，即假定了这个子树中只有一个 `p` 节点或 `q` 节点，因此其他公共祖先不会再被判断为符合条件。

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
    public TreeNode ans;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        dfs(root, p, q);
        return ans;
    }
    private boolean dfs(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return false;
        boolean lson = dfs(root.left, p, q);
        boolean rson = dfs(root.right, p, q);
        if ((lson && rson) || ((root.val == p.val || root.val == q.val) && (lson || rson))) {
            ans = root;
        } 
        return lson || rson || (root.val == p.val || root.val == q.val);
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(N)$，其中 `N` 是二叉树的节点数。二叉树的所有节点有且只会被访问一次，因此时间复杂度为 $O(N)$。

- 空间复杂度：$O(N)$，其中 `N` 是二叉树的节点数。递归调用的栈深度取决于二叉树的高度，二叉树最坏情况下为一条链，此时高度为 `N`，因此空间复杂度为 $O(N)$。
### 解法二:哈希表存储双亲节点
我们可以用哈希表存储所有节点的父节点，然后我们就可以利用节点的父节点信息从 `p` 结点开始不断往上跳，并记录已经访问过的节点，再从 `q` 节点开始不断往上跳，如果碰到已经访问过的节点，那么这个节点就是我们要找的最近公共祖先。

算法：

1. 从根节点开始遍历整棵二叉树，用哈希表记录每个节点的父节点指针。
2. 从 `p` 节点开始不断往它的祖先移动，并用数据结构记录已经访问过的祖先节点。
3. 同样，我们再从 `q` 节点开始不断往它的祖先移动，如果有祖先已经被访问过，即意味着这是 `p` 和 `q` 的深度最深的公共祖先，即 `LCA` 节点。
   
```java
class Solution {
    Map<Integer, TreeNode> parent = new HashMap<Integer, TreeNode>();
    Set<Integer> visited = new HashSet<Integer>();

    public void dfs(TreeNode root) {
        if (root.left != null) {
            parent.put(root.left.val, root);
            dfs(root.left);
        }
        if (root.right != null) {
            parent.put(root.right.val, root);
            dfs(root.right);
        }
    }

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        dfs(root);
        while (p != null) {
            visited.add(p.val);
            p = parent.get(p.val);
        }
        while (q != null) {
            if (visited.contains(q.val)) {
                return q;
            }
            q = parent.get(q.val);
        }
        return null;
    }
}
```
#### 复杂度分析

- 时间复杂度：$O(N)$，其中 `N` 是二叉树的节点数。二叉树的所有节点有且只会被访问一次，从 `p` 和 `q` 节点往上跳经过的祖先节点个数不会超过 `N`，因此总的时间复杂度为 $O(N)$。

- 空间复杂度：$O(N)$ ，其中 `N` 是二叉树的节点数。递归调用的栈深度取决于二叉树的高度，二叉树最坏情况下为一条链，此时高度为 `N`，因此空间复杂度为 $O(N)$，哈希表存储每个节点的父节点也需要 $O(N)$ 的空间复杂度，因此最后总的空间复杂度为 $O(N)$。

### 解法三：暴力
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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        List<TreeNode> listP=new ArrayList<>();
        List<TreeNode> listQ=new ArrayList<>();
        helper(root,p,listP);
        helper(root,q,listQ);
        for(TreeNode tempP:listP){
            for(TreeNode tempQ:listQ){
                if(tempP==tempQ){
                    return tempP;
                }
            }
        }
        return null;
    }
    public boolean helper(TreeNode root,TreeNode temp,List<TreeNode> list){
        if(root==null){
            return false;
        }
        if(root==temp){
            list.add(root);
            return true;
        }
        if(helper(root.left,temp,list)||helper(root.right,temp,list)){
            list.add(root);
            return true;
        }
        return false;
    }
}
```





