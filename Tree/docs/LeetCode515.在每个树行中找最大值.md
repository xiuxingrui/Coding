# [LeetCode515.在每个树行中找最大值](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)
## 题目描述
您需要在二叉树的每一行中找到最大的值。

### 示例
```
输入: 

          1
         / \
        3   2
       / \   \  
      5   3   9 

输出: [1, 3, 9]
```
## 题解
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
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> ans=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        if(root!=null){
            queue.offer(root);
        }
        while(!queue.isEmpty()){
            int size=queue.size();
            int maxValue=Integer.MIN_VALUE;
            for(int i=0;i<size;i++){
                TreeNode tempNode=queue.poll();
                if(tempNode.val>maxValue){
                    maxValue=tempNode.val;
                }
                if(tempNode.left!=null){
                    queue.offer(tempNode.left);
                }
                if(tempNode.right!=null){
                    queue.offer(tempNode.right);
                }
            }
            ans.add(maxValue);
        }
        return ans;
    }
}
```