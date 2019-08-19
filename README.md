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


----


## 100. Same Tree
秒杀


---


## 101. Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).


### Solution:
#### recursive
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
    public boolean isSymmetric(TreeNode root) {
        if(root==null){
            return true;
        }
        return helper(root.left,root.right);
    }
    
    public boolean helper(TreeNode left, TreeNode right){
        if(left!=null&&right!=null){
            return left.val==right.val&&helper(left.left,right.right)
                &&helper(left.right,right.left);
        }else if(left==null&&right==null){
            return true;
        }
        return false;
    }
}
```

### iterative
```
public boolean isSymmetric(TreeNode root) {
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);
    q.add(root);
    while (!q.isEmpty()) {
        TreeNode t1 = q.poll();
        TreeNode t2 = q.poll();
        if (t1 == null && t2 == null) continue;
        if (t1 == null || t2 == null) return false;
        if (t1.val != t2.val) return false;
        q.add(t1.left);
        q.add(t2.right);
        q.add(t1.right);
        q.add(t2.left);
    }
    return true;
}
```


---


## 104. Maximum Depth of Binary Tree
秒杀


---


## 105. Construct Binary Tree from Preorder and Inorder Traversal


Given preorder and inorder traversal of a tree, construct the binary tree.


### Solution:


```
class Solution {
  // start from first preorder element
  int pre_idx = 0;
  int[] preorder;
  int[] inorder;
  HashMap<Integer, Integer> idx_map = new HashMap<Integer, Integer>();

  public TreeNode helper(int in_left, int in_right) {
    // if there is no elements to construct subtrees
    if (in_left == in_right)
      return null;

    // pick up pre_idx element as a root
    int root_val = preorder[pre_idx];
    TreeNode root = new TreeNode(root_val);

    // root splits inorder list
    // into left and right subtrees
    int index = idx_map.get(root_val);

    // recursion 
    pre_idx++;
    // build left subtree
    root.left = helper(in_left, index);
    // build right subtree
    root.right = helper(index + 1, in_right);
    return root;
  }

  public TreeNode buildTree(int[] preorder, int[] inorder) {
    this.preorder = preorder;
    this.inorder = inorder;

    // build a hashmap value -> its index
    int idx = 0;
    for (Integer val : inorder)
      idx_map.put(val, idx++);
    return helper(0, inorder.length);
  }
}
```


### Tips:
- use a hashmap to record index position to avoid iteration every recursion.


---


## 106. Construct Binary Tree from Inorder and Postorder Traversal


Given inorder and postorder traversal of a tree, construct the binary tree.


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
    HashMap<Integer,Integer> index = new HashMap();
    int _postindex;
    int[] inorder;
    int[] postorder;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder.length==0){return null;}
        _postindex = postorder.length-1;
        this.inorder = inorder;
        this.postorder = postorder;
        for(int i=0;i<inorder.length;i++){
            index.put(inorder[i],i);
        }
        return helper(0,inorder.length);
    }
    
    public TreeNode helper(int instart, int inend){
        if(instart==inend){return null;}
        TreeNode root = new TreeNode(postorder[_postindex]);
        int i = index.get(postorder[_postindex]);
        _postindex--;
        root.right = helper(i+1,inend);
        root.left = helper(instart,i);
        return root;
    }
}
```


---
