# Segment Tree Modify
> For a Maximum Segment Tree, which each node has an extra value max to store the maximum value in this node's interval.
>
> Implement a modify function with three parameter root, index and value to change the node's value with [start, end] = [index, index] to the new given value. Make sure after this change, every node in segment tree still has the max attribute with the correct value.
>
> Example
>
> For segment tree:
> 
>                          [1, 4, max=3]
>                       /                \
>           [1, 2, max=2]                [3, 4, max=3]
>          /              \             /             \
>     [1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=3]
>
> if call modify(root, 2, 4), we can get:
>
>                           [1, 4, max=4]
>                        /                \
>            [1, 2, max=4]                [3, 4, max=3]
>          /              \             /             \
>     [1, 1, max=2], [2, 2, max=4], [3, 3, max=0], [4, 4, max=3]
>
> or call modify(root, 4, 0), we can get:
>
>                         [1, 4, max=2]
>                       /                \
>           [1, 2, max=2]                [3, 4, max=0]
>          /              \             /             \
>      [1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=0]

### Complexity
Time: O(h)

Space: O(1)

### Code
#### Java
```java
/**
 * Definition of SegmentTreeNode:
 * public class SegmentTreeNode {
 *     public int start, end, max;
 *     public SegmentTreeNode left, right;
 *     public SegmentTreeNode(int start, int end, int max) {
 *         this.start = start;
 *         this.end = end;
 *         this.max = max
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
  /**
   *@param root, index, value: The root of segment tree and 
   *@ change the node's value with [index, index] to the new given value
   *@return: void
   */
  public void modify(SegmentTreeNode root, int index, int value) {
    if (root == null || index > root.end || index < root.start) {
      return;
    }
    if (root.start == index && root.end == index) {
      root.max = value;
      return;
    }
    modify(root.left, index, value);
    modify(root.right, index, value);
    int leftMax = root.left == null ? Integer.MIN_VALUE : root.left.max;
    int rightMax = root.right == null ? Integer.MIN_VALUE : root.right.max;
    root.max = Math.max(leftMax, rightMax);
  }
}
```
