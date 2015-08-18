# Subtree
> You have two every large binary trees: T1, with millions of nodes, and T2, with hundreds of nodes. Create an algorithm to decide if T2 is a subtree of T1.
>
> Example
>
> T2 is a subtree of T1 in the following case:
>
>            1                3
>           / \              / 
>     T1 = 2   3      T2 =  4
>         /
>        4
>
> T2 isn't a subtree of T1 in the following case:
>
>             1               3
>            / \               \
>      T1 = 2   3       T2 =    4
>          /
>         4
>
> Note
>
> A tree T2 is a subtree of T1 if there exists a node n in T1 such that the subtree of n is identical to T2. That is, if you cut off the tree at node n, the two trees would be identical.

## Method 1: Divide and Conquer
### Analysis
Recursively check each node as the root of the potential identical subtree and compare it with T2. It is actually a recursive in-order traversal.

### Complexity
Time: O(m * n)

Space: (h)

### Code
#### Java
```java
public class Solution {
  public boolean isSubtree(TreeNode T1, TreeNode T2) {
    if (T2 == null) {
      return true;
    } else if (T1 == null) {
      return false;
    }
    return isSameTree(T1, T2) || isSubtree(T1.left, T2) || isSubtree(T1.right, T2);
  }

  private boolean isSameTree(TreeNode root1, TreeNode root2) {
    if (root1 == null && root2 == null) {
      return true;
    } else if (root1 == null || root2 == null) {
      return false;
    } else if (root1.val != root2.val) {
      return false;
    }
    return isSameTree(root1.left, root2.left) && isSameTree(root1.right, root2.right);
  }
}
```
