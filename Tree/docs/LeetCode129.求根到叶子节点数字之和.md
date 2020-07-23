# [LeetCode129.求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)
## 题目描述
给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径 1->2->3 代表数字 123。

计算从根到叶子节点生成的所有数字之和。



### 示例
```
输入: [1,2,3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
```
```
输入: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
输出: 1026
解释:
从根到叶子节点路径 4->9->5 代表数字 495.
从根到叶子节点路径 4->9->1 代表数字 491.
从根到叶子节点路径 4->0 代表数字 40.
因此，数字总和 = 495 + 491 + 40 = 1026.
```
## 题解
### 解法一
写法一：
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
    int sum=0;
    public int sumNumbers(TreeNode root) {
        backtracking(root,0);
        return sum;
    }
    public void backtracking(TreeNode root,int num){
        if(root==null){
            return;
        }
        num=num*10+root.val;
        if(root.left==null&&root.right==null){
            sum+=num;
            return;
        }
        backtracking(root.left,num);
        backtracking(root.right,num);
    }
}
```
写法二:
```java
    public int sumNumbers(TreeNode root) {
        return helper(root, 0);
    }

    public int helper(TreeNode root, int i){
        if (root == null) return 0;
        int temp = i * 10 + root.val;
        if (root.left == null && root.right == null)
            return temp;
        return helper(root.left, temp) + helper(root.right, temp);
    }
```

### 解法二:栈
```java
class Solution {
    public int sumNumbers(TreeNode root) {
        int sum = 0;
        if (root == null) return sum;
        Deque<TreeNode> nodeStack = new LinkedList<>();
        Deque<Integer> numStack = new LinkedList<>();
        nodeStack.add(root);
        numStack.add(0);
        while (!nodeStack.isEmpty()) {
            TreeNode current = nodeStack.pop();
            Integer currentNum = numStack.pop() * 10 + current.val;
            if (current.left == null && current.right == null) {
                sum += currentNum;
            }
            if (current.left != null) {
                nodeStack.push(current.left);
                numStack.push(currentNum);
            }
            if (current.right != null) {
                nodeStack.push(current.right);
                numStack.push(currentNum);
            }
        } 
        return sum;
    }
}
```
### 解法三:队列
```java
public class Solution {
    public int sumNumbers(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        Queue<Integer> numQueue = new LinkedList<Integer>();
        if(root == null) return 0;
        int res = 0;
        queue.add(root);
        numQueue.add(0);
        while(!queue.isEmpty()) {
            int size = queue.size();
            // 把该层的都入队，同时如果遇到叶节点，计算更新
            while(size-- > 0) {
                root = queue.poll();
                int val = numQueue.poll() * 10 + root.val;
                if(root.left == null && root.right == null)
                    res += val;
                if(root.left != null) {
                    queue.add(root.left);
                    numQueue.add(val);
                }
                if (root.right != null) {
                    queue.add(root.right);
                    numQueue.add(val);
                }
            }
        }
        return res;
    }
}
```


