<!-- TOC -->

- [广度优先搜索](#广度优先搜索)
  - [模板](#模板)
    - [leetcode102下解答](#leetcode102下解答)
  - [树](#树)
    - [简单](#简单)
    - [中等](#中等)

<!-- /TOC -->
# 广度优先搜索
## 模板
### leetcode102下解答
1. 如果不需要确定当前遍历到了哪一层，BFS模板:
```
while queue 不空：
    cur = queue.pop()
    for 节点 in cur的所有相邻节点：
        if 该节点有效且未访问过：
            queue.push(该节点)
```
2. 如果要确定当前遍历到了哪一层，BFS模板:
```
level = 0
while queue 不空：
    size = queue.size()
    while (size --) {
        cur = queue.pop()
        for 节点 in cur的所有相邻节点：
            if 该节点有效且未被访问过：
                queue.push(该节点)
    }
    level ++;
```
## 树
### 简单
- [LeetCode111.二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
- [LeetCode101.对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)
- [剑指offer32-I.从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)
### 中等
- [LeetCode102.二叉树的层序遍历/剑指offer32-II.从上到下打印二叉树II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
- [LeetCode103.二叉树的锯齿形层次遍历/剑指offer32-III.从上到下打印二叉树III](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)