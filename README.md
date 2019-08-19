# LeetCode
LeetCode review

# DFS


## 98. Validate Binary Search Tree


Given a binary tree, determine if it is a valid binary search tree (BST).


Assume a BST is defined as follows:



The left subtree of a node contains only nodes with keys less than the node's key.


The right subtree of a node contains only nodes with keys greater than the node's key.


Both the left and right subtrees must also be binary search trees.


### Solution:


```
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
    public boolean isValidBST(TreeNode root) {
        if(root!=null){
            boolean res = true;
            if(root.left!=null){
                TreeNode leftmax = root.left;
                while(leftmax.right!=null){
                    leftmax = leftmax.right;
                }
                res = res&&root.val>root.left.val&&root.val>leftmax.val;
            }
            if(root.right!=null){
                TreeNode rightmin = root.right;
                while(rightmin.left!=null){
                    rightmin = rightmin.left;
                }
                res = res&&root.val<root.right.val&&root.val<rightmin.val;
            }
            return res&&isValidBST(root.left)&&isValidBST(root.right);
        }
        return true;
    }
}
```


### Mistake:
- forgot that the largest value in left subtree should be also larger than root value.(same as right side).


-----
