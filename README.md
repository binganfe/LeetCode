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


## 99. Recover Binary Search Tree


Two elements of a binary search tree (BST) are swapped by mistake.


Recover the tree without changing its structure.


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
    public void recoverTree(TreeNode root) {
        if(root!=null){
            TreeNode[] eles = new TreeNode[2];
            boolean[] flags = {true,true,false};
            inorder(root,eles,new TreeNode[1],flags);
            int tmp = eles[1].val;
            eles[1].val = eles[0].val;
            eles[0].val = tmp;
        }
    }
    
    public void inorder(TreeNode root, TreeNode[] eles, TreeNode[] prev, 
                        boolean[] flags){
        if(root!=null){
            inorder(root.left,eles,prev,flags);
            if(flags[0]){
                prev[0] = root;
                flags[0] =false;
            }else{
                if(prev[0].val>root.val){
                    if(flags[1]){
                        eles[0] = prev[0];
                        eles[1] = root;
                        flags[1] = false;
                    }else{
                        eles[1] = root;
                        flags[2] = true;
                    }
                }
                prev[0] = root;
            }
            if(!flags[2]){
                inorder(root.right,eles,prev,flags);
            }
        }
    }
}
```


### Tips:
- Use an array to store information in recursion to avoid the difficulty of getting information by just return.


### Mistake:
- Only Object can keep the change in recursion.


