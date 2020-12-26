# [剑指Offer32-III.从上到下打印二叉树III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)
## 题目描述
请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

例如:
给定二叉树: `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
返回其层次遍历结果：
```
[
  [3],
  [20,9],
  [15,7]
]
```

提示：

- `节点总数 <= 1000`
## 题解
### 解法一
链表：
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        if(root!=null){
            queue.offer(root);
        }
        while(!queue.isEmpty()){
            LinkedList<Integer> list=new LinkedList<>();
            int size=queue.size();
            for(int i=0;i<size;i++){
                TreeNode node=queue.poll();
                if(ans.size()%2==0){
                    list.addLast(node.val);
                }else{
                    list.addFirst(node.val);
                }
                if(node.left!=null){
                    queue.offer(node.left);
                }
                if(node.right!=null){
                    queue.offer(node.right);
                }
            }
            ans.add(list);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 为二叉树的节点数。每个节点会且仅会被遍历一次。
- 空间复杂度：$O(N)$。
### 解法二
双端队列：
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans=new ArrayList<>();
        Deque<TreeNode> deque=new LinkedList<>();
        if(root!=null){
            deque.offer(root);
        }
        while(!deque.isEmpty()){
            List<Integer> list=new ArrayList<>();
            int size=deque.size();
            if(ans.size()%2==0){
                for(int i=0;i<size;i++){
                    TreeNode node=deque.pollFirst();
                    list.add(node.val);
                    if(node.left!=null){
                        deque.offerLast(node.left);
                    }
                    if(node.right!=null){
                        deque.offerLast(node.right);
                    }
                }
            }else{
                for(int i=0;i<size;i++){
                    TreeNode node=deque.pollLast();
                    list.add(node.val);
                    if(node.right!=null){
                        deque.offerFirst(node.right);
                    }
                    if(node.left!=null){
                        deque.offerFirst(node.left);
                    }
                }
            }
            ans.add(list);
        }
        return ans;
    }
}
```
#### 复杂度分析
- 时间复杂度：$O(N)$，其中 `N` 为二叉树的节点数。每个节点会且仅会被遍历一次。
- 空间复杂度：$O(N)$。我们需要维护存储节点的队列和存储节点值的双端队列，空间复杂度为 $O(N)$。