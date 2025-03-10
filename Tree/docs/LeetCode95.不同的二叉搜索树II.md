# [LeetCode95.不同的二叉搜索树II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)
## 题目描述
给定一个整数 `n`，生成所有由 `1...n` 为节点所组成的 二叉搜索树 。

- `0 <= n <= 8`
### 示例
```
输入：3
输出：
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
解释：
以上的输出对应以下 5 种不同结构的二叉搜索树：

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
## 题解
构建一颗二叉搜索树

构建一颗二叉搜索树很简单，只需要选择一个根结点，然后递归去构建其左右子树。

```java
public TreeNode createBinaryTree(int n){
    return helper(1, n);
}

public TreeNode helper(int start, int end){
    if(start > end)
        return null;

    // 这里可以选择从start到end的任何一个值做为根结点，
    // 这里选择它们的中点，实际上，这样构建出来的是一颗平衡二叉搜索树
    int val = (start + end) / 2;
    TreeNode root = new TreeNode(val);

    root.left = helper(start, val - 1);
    root.right = helper(val + 1, end);

    return root;
}
```
要构建多颗二叉树，问题就在于如何选择不同的根节点，以构建不同的树和子树。

在上面的代码中，在选择根结点的时候，可以这样改造

```java
// 选择所有可能的根结点
for(int i = start; i <= end; i++){
    TreeNode root = new TreeNode(i);
    ...
}
```
但是如果按照上述递归函数的方法写，每次递归只能返回一颗树，我们需要的是多颗树，我们可以将不同的根结点装入`List`然后返回，实际上，上述代码可以改写成

```java
public List<TreeNode> helper(int start, int end){
    List<TreeNode> list = new ArrayList<>();        

    if(start > end){
        list.add(null);
        return list;
    }

    for(int i = start; i <= end; i++){
        TreeNode root = new TreeNode(i);
        ...
        ...
        // 装入所有根结点
        list.add(root);
    }

    return list;
}
```
很显然，现在问题变成了如何构建`root`的左右子树，我们抛开复杂的递归函数，只关心递归的返回值，每次选择根结点`root`，我们

- 递归构建左子树，并拿到左子树所有可能的根结点列表`left`
- 递归构建右子树，并拿到右子树所有可能的根结点列表`right`
- 
这个时候我们有了左右子树列表，我们的左右子树都是各不相同的，因为根结点不同，我们如何通过左右子树列表构建出所有的以`root`为根的树呢？

我们固定一个左孩子，遍历右子树列表，那么以当前为`root`根结点的树个数就为`left.size() * right.size()`个。

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if(n < 1)
            return new ArrayList<>();
        return helper(1, n);
    }

    public List<TreeNode> helper(int start, int end){
        List<TreeNode> list = new ArrayList<>();

        if(start > end){
            // 如果当前子树为空，不加null行吗？
            list.add(null);
            return list;
        }

        for(int i = start; i <= end; i++){
            // 想想为什么这行不能放在这里，而放在下面？
            // TreeNode root = new TreeNode(i);
            List<TreeNode> left = helper(start, i-1);  
            List<TreeNode> right = helper(i+1, end); 

            // 固定左孩子，遍历右孩子
            for(TreeNode l : left){
                for(TreeNode r : right){
                    TreeNode root = new TreeNode(i);
                    root.left = l;
                    root.right = r;
                    list.add(root);
                }
            }
        }
        return list;
    }
}
```
关于`TreeNode root = new TreeNode(i)`的放置的位置问题:

如果这行代码放置在注释的地方，会造成一个问题，就是以当前为`root`根结点的树个数就`num = left.size() * right.size() > 1`时，`num`棵子树会共用这个`root`结点，在下面两层`for`循环中，`root`的左右子树一直在更新，如果每次不新建一个`root`，就会导致`num`个`root`为根节点的树都相同。

关于如果当前子树为空，不加`null`行不行的问题:

显然，如果一颗树的左子树为空，右子树不为空，要正确构建所有树，依赖于对左右子树列表的遍历，也就是上述代码两层for循环的地方，如果其中一个列表为空，那么循环都将无法进行。
### 复杂度分析
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201013150253.png)
