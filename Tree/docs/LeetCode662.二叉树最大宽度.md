# [LeetCode662.二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)
## 题目描述
给定一个二叉树，编写一个函数来获取这个树的最大宽度。树的宽度是所有层中的最大宽度。这个二叉树与满二叉树（`full binary tree`）结构相同，但一些节点为空。

每一层的宽度被定义为两个端点（该层最左和最右的非空节点，两端点间的`null`节点也计入长度）之间的长度。

注意: 答案在32位有符号整数的表示范围内。
### 示例
```
输入: 

           1
         /   \
        3     2
       / \     \  
      5   3     9 

输出: 4
解释: 最大值出现在树的第 3 层，宽度为 4 (5,3,null,9)。
```
```
输入: 

          1
         /  
        3    
       / \       
      5   3     

输出: 2
解释: 最大值出现在树的第 3 层，宽度为 2 (5,3)。
```
```
输入: 

          1
         / \
        3   2 
       /        
      5      

输出: 2
解释: 最大值出现在树的第 2 层，宽度为 2 (3,2)。
```
```
输入: 

          1
         / \
        3   2
       /     \  
      5       9 
     /         \
    6           7
输出: 8
解释: 最大值出现在树的第 4 层，宽度为 8 (6,null,null,null,null,null,null,7)。
```
## 题解
想象一下用数组存储树中结点。将二叉树的下标存储到数组中，根节点下标为 0，左子树结点为 `2*i+1`，右子树下标为 `2*i+2`。然后我们层次遍历，每次比较最大宽度值。
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        Queue<TreeNode> queue=new LinkedList<>();
        Queue<Integer> index=new LinkedList<>();
        int max=0;
        if(root==null){
            return 0;
        }
        queue.offer(root);
        index.offer(0);
        while(!queue.isEmpty()){
            int size=queue.size();
            int start=0,end=0;
            for(int i=0;i<size;i++){
                TreeNode temp=queue.poll();
                int curIndex=index.poll();
                if(i==0){
                    start=curIndex;
                }
                if(i==size-1){
                    end=curIndex;
                }
                if(temp.left!=null){
                    queue.offer(temp.left);
                    index.offer(curIndex*2+1);
                }
                if(temp.right!=null){
                    queue.offer(temp.right);
                    index.offer(curIndex*2+2);
                }
            }
            max=Math.max(max,end-start+1);
        }
        return max;
    }
}
```
### 复杂度分析
- 时间复杂度： $O(N)$，其中 `N` 是输入树的节点数目，我们遍历每个节点一遍。
- 空间复杂度： $O(N)$，这是 `queue` 的大小。

自己的写法:超时
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        if(root==null){
            return 0;
        }
        Queue<TreeNode> queue=new LinkedList<>();
        queue.offer(root);
        int max=0;
        while(!queue.isEmpty()){
            int size=queue.size();
            int start=0,last=0;
            boolean flag=false,flag1=false;
            for(int i=0;i<size;i++){
                TreeNode temp=queue.poll();
                if(temp!=null){
                    if(flag==false){
                        start=i;
                        flag=true;
                    }else{
                        last=i;
                    }
                    queue.offer(temp.left);
                    if(temp.left!=null){
                        flag1=true;
                    }
                    queue.offer(temp.right);
                    if(temp.right!=null){
                        flag1=true;
                    }
                }else{
                    queue.offer(null);
                    queue.offer(null);
                }
            }
            max=Math.max(max,last-start+1);
            if(flag1==false){
                break;
            }
        }
        return max;
    }
}
```
