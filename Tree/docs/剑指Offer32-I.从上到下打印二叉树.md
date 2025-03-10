# [剑指Offer32-I.从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)
## 题目描述
从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

例如:
给定二叉树: `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```
返回：
```
[3,9,20,15,7]
```
提示：

- `节点总数 <= 1000`

## 题解
```java
class Solution {
    public int[] levelOrder(TreeNode root) {
        if(root == null) return new int[0];
        Queue<TreeNode> queue = new LinkedList<>(){{ add(root); }};
        ArrayList<Integer> ans = new ArrayList<>();
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            ans.add(node.val);
            if(node.left != null) queue.add(node.left);
            if(node.right != null) queue.add(node.right);
        }
        int[] res = new int[ans.size()];
        for(int i = 0; i < ans.size(); i++)
            res[i] = ans.get(i);
        return res;
    }
}
```
### 复杂度分析
- 时间复杂度 $O(N)$ ： `N` 为二叉树的节点数量，即 `BFS` 需循环 `N` 次。
- 空间复杂度 $O(N)$ ： 最差情况下，即当树为平衡二叉树时，最多有 `N/2` 个树节点同时在 `queue` 中，使用 $O(N)$ 大小的额外空间。