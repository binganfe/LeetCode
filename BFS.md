# LeetCode
LeetCode review

# BFS


## 102. Binary Tree Level Order Traversal
秒杀


---


## 103. Binary Tree Zigzag Level Order Traversal
秒杀


---


## 107. Binary Tree Level Order Traversal II
秒杀


---


## 111. Minimum Depth of Binary Tree
Given a binary tree, find its minimum depth.


The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.


Note: A leaf is a node with no children.


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
    public int minDepth(TreeNode root) {
        if(root!=null){
            if(root.left!=null&&root.right!=null){
                return 1+Math.min(minDepth(root.left),minDepth(root.right));
            }else if(root.left==null){
                return 1+minDepth(root.right);
            }else{
                return 1+minDepth(root.left);
            }
        }
        return 0;
    }
}
```


### Mistake:
- Depth should be down to leaf node.
---
