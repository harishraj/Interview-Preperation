# Binary Tree Upside Down
### Source
1. [leetcode](https://leetcode.com/problems/binary-tree-upside-down/)

> Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.
> 
> For example:
>
> Given a binary tree {1,2,3,4,5},
>         1
>        / \
>       2   3
>      / \
>     4   5
>
> return the root of the binary tree [4,5,2,#,#,3,1].
>
>       4
>      / \
>     5   2
>        / \
>       3   1  

## Method 1: Divide and Conquer
### Analysis
Since the right child cannot have any other children, we can recursively construct the result. 

### Complexity
Time: O(N)

Space: O(logN)

### Code
#### Java
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
public class Solution {
  public TreeNode upsideDownBinaryTree(TreeNode root) {
    if (root == null || root.left == null) {
      return root;
    }
    TreeNode leftChild = root.left;
    TreeNode rightChild = root.right;
    TreeNode newRoot = upsideDownBinaryTree(root.left);
    leftChild.left = rightChild;
    leftChild.right = root;
    root.left = null;
    root.right = null;
    return newRoot;
  }
}
```

## Method 2: Iterative
### Analysis
Set the current node's left child to its right sibling, and set its right child to its parent. 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
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
public class Solution {
  public TreeNode upsideDownBinaryTree(TreeNode root) {
    if (root == null) {
      return root;
    }
    TreeNode cur = root;
    TreeNode parent = null;
    TreeNode rightSibling = null;
    while (cur != null) {
      TreeNode next = cur.left;
      cur.left = rightSibling;
      rightSibling = cur.right;
      cur.right = parent;
      parent = cur;
      cur = next;
    }
    return parent;
  }
}
```
