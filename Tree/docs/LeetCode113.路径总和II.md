# [LeetCode113.路径总和II](https://leetcode-cn.com/problems/path-sum-ii/)
## 题目描述
给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

### 示例
给定如下二叉树，以及目标和 `sum = 22`，
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
返回:
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```
## 题解
```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> result = new ArrayList<>();
        recur(result, new ArrayList<Integer>(), root, sum);
        return result;
    }

    private void recur(List<List<Integer>> result, List<Integer> curPath, TreeNode curNode, int sum){
        if(curNode == null){
            return;
        }
        curPath.add(curNode.val);
        if(curNode.val == sum && curNode.left == null && curNode.right == null){
            result.add(new ArrayList<>(curPath));
        }
        recur(result, curPath, curNode.left, sum - curNode.val);
        recur(result, curPath, curNode.right, sum - curNode.val);
        curPath.remove(curPath.size() - 1);
    }
}
```

