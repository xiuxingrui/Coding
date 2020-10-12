# [剑指Offer37.序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)
## 题目描述
请实现两个函数，分别用来序列化和反序列化二叉树。

### 示例
```
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```
## 题解
观察题目示例，序列化的字符串实际上是二叉树的 “层序遍历”（`BFS`）结果，本文也采用层序遍历。

通常使用的前序、中序、后序、层序遍历记录二叉树的信息不完整，即可能对应着多种二叉树结果。

题目要求的 “序列化” 和 “反序列化” 是 可逆 操作。因此，序列化的字符串应携带 “完整的” 二叉树信息，即拥有单独表示二叉树的能力。

为使反序列化可行，考虑将越过叶节点后的 `null` 也看作是节点。在此基础上，对于列表中任意某节点 `node`，其左子节点 `node.left`和右子节点 `node.right` 在序列中的位置都是 唯一确定 的。

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201012141239.png)

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201012141308.png)

设 `m` 为列表区间 `[0,n]` 中空节点（即 `null` ）的个数，则可总结出 `node,node.left,node.right`在列表索引的对应关系：

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20201012141435.png)

序列化 `serialize` ：

算法流程：

- 特例处理： 若 `root` 为空，则直接返回空列表 `"[]"` ；
- 初始化： 队列 `queue` （包含根节点 `root` ）；序列化列表 `res` ；
- 层序遍历： 当 `queue`为空时跳出；
- 节点出队，记为 `node`；

若 `node` 不为空：① 打印字符串 `node.val`，② 将左、右子节点加入 `queue`；
否则（若 `node` 为空）：打印字符串 `"null"` ；

返回值： 拼接列表（用 `','` 隔开，首尾添加中括号）。

```java
public String serialize(TreeNode root) {
        if(root == null) return "[]";
        StringBuilder res = new StringBuilder("[");
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node != null) {
                res.append(node.val + ",");
                queue.add(node.left);
                queue.add(node.right);
            }
            else res.append("null,");
        }
        res.deleteCharAt(res.length() - 1);
        res.append("]");
        return res.toString();
```
复杂度分析：
- 时间复杂度 $O(N)$ ： `N` 为二叉树的节点数，层序遍历需要访问所有节点，最差情况下需要访问 `N+1` 个 `null`，总体复杂度为 $O(2N+1)=O(N)$
- 空间复杂度 $O(N)$： 最差情况下，队列 `queue` 同时存储 `N+1/2`个节点（或 `N+1`个 `null`），使用 $O(N)$；列表 `res` 使用 $O(N)$。

反序列化 `deserialize` ：

基于本文一开始分析的 `“node,node.left,node.right”` 在序列化列表中的位置关系，可实现反序列化。(其实感觉并没有用到位置关系)

利用队列按层构建二叉树，借助一个指针 `i` 指向节点 `node`的左、右子节点，每构建一个 `node`的左、右子节点，指针 `i` 就向右移动1位。

算法流程：

- 特例处理： 若 `data` 为空，直接返回 `null`；
- 初始化： 序列化列表 `vals`（先去掉首尾中括号，再用逗号隔开），指针 `i=1`，根节点 `root` （值为 `vals[0]`），队列 `queue`（包含 `root`）；
- 按层构建： 当 `queue` 为空时跳出；
- 节点出队，记为 `node`；
- 构建 `node` 的左子节点：`node.left`的值为 `vals[i]`，并将 `node.left`入队；
- 执行 `i+=1`；
- 构建 `node`的右子节点：`node.left`的值为 `vals[i]`，并将 `node.left`入队；
- 执行 `i+=1` ；
返回值： 返回根节点 `root`即可。

```java
public TreeNode deserialize(String data) {
        if(data.equals("[]")) return null;
        String[] vals = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        int i = 1;
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(!vals[i].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.left);
            }
            i++;
            if(!vals[i].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.right);
            }
            i++;
        }
        return root;
    }
```
复杂度分析：
- 时间复杂度 $O(N)$ ： `N` 为二叉树的节点数，按层构建二叉树需要遍历整个 `vals` ，其长度最大为 `2N+1` 。
- 空间复杂度 $O(N)$： 最差情况下，队列 `queue` 同时存储 `N+1/2`个节点（或 `N+1`个 `null`），使用 $O(N)$；

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
public class Codec {
    public String serialize(TreeNode root) {
        if(root == null) return "[]";
        StringBuilder res = new StringBuilder("[");
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node != null) {
                res.append(node.val + ",");
                queue.add(node.left);
                queue.add(node.right);
            }
            else res.append("null,");
        }
        res.deleteCharAt(res.length() - 1);
        res.append("]");
        return res.toString();
    }

    public TreeNode deserialize(String data) {
        if(data.equals("[]")) return null;
        String[] vals = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        int i = 1;
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(!vals[i].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.left);
            }
            i++;
            if(!vals[i].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.right);
            }
            i++;
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```