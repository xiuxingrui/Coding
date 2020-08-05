# [LeetCode1214.查找两棵二叉搜索树之和](https://leetcode-cn.com/problems/two-sum-bsts/)
## 题目描述
给出两棵二叉搜索树，请你从两棵树中各找出一个节点，使得这两个节点的值之和等于目标值 `Target`。

如果可以找到返回 `True`，否则返回 `False`。

提示：
- 每棵树上最多有 5000 个节点。
- `-10^9 <= target, node.val <= 10^9`

![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200805135743.png)
```
输入：root1 = [2,1,4], root2 = [1,0,3], target = 5
输出：true
解释：2 加 3 和为 5 。
```
![](https://picgp.oss-cn-beijing.aliyuncs.com/img/20200805135802.png)

```
输入：root1 = [0,-10,10], root2 = [5,1,7,0,2], target = 18
输出：false
```
## 题解
思路：
1. 遍历其中一棵`BST1`,遍历到每个结点的时候，取出该结点值`node.val`，求得`(target-node.val)`的值拿到`BST2`去查找，找到就返回`true`。
2. 若同步步骤1没找到，则递归遍历`BST1`的左子树和`BST1`的右子树，不断执行步骤1，直到找到或者所有节点遍历完也未找到为止。

### 解法一
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
    private boolean find(TreeNode root, int value) {
        if (root == null) {
            return false;
        }

        if (root.val == value) {
            return true;
        } else if (root.val < value) {
            return find(root.right, value);
        } else {
            return find(root.left, value);
        }
    }

    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
        if (root1 == null) {
            return false;
        }

        // 使用或运算进行短路操作，找到就终止
        return find(root2, target - root1.val) || twoSumBSTs(root1.left, root2, target) ||
                twoSumBSTs(root1.right, root2, target);
    }
}
```
### 解法二
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
    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
        List<Integer> list1=new ArrayList<>();
        List<Integer> list2=new ArrayList<>();
        inorder(root1,list1);
        inorder(root2,list2);
        int[] nums1=new int[list1.size()];
        int[] nums2=new int[list2.size()];
        int cnt=0;
        for(int num:list1){
            nums1[cnt++]=num;
        }
        cnt=0;
        for(int num:list2){
            nums2[cnt++]=num;
        }
        int left=0,right=cnt-1;
        while(left<=nums1.length-1&&right>=0){
            if(nums1[left]+nums2[right]==target){
                return true;
            }else if(nums1[left]+nums2[right]<target){
                left++;
            }else{
                right--;
            }
        }
        return false;
    }
    public void inorder(TreeNode root,List<Integer> list){
        if(root==null){
            return;
        }
        inorder(root.left,list);
        list.add(root.val);
        inorder(root.right,list);
    }
}
```
