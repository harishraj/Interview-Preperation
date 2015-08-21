# Count of Smaller Number Before Itself
> Give you an integer array (index from 0 to n-1, where n is the size of this array, value from 0 to 10000) . For each element Ai in the array, count the number of element before this element Ai is smaller than it and return count number array.
>
> Example
>
> For array [1,2,7,8,5], return [0,1,2,3,2]

## Method 1: Segment Tree
### Analysis
Almost the same as [Count of Smaller Number](count_of_smaller_number.md).

### Complexity
Time: O(nlgn)

Space: o(n)

### Code
#### Java
```java
public class Solution {
  public ArrayList<Integer> countOfSmallerNumberII(int[] A) {
    ArrayList<Integer> result = new ArrayList<>();
    SegmentTreeNode root = buildSegmentTree(0, 10000);
    for (int num : A) {
      modifySegmentTree(root, num, 1);
      result.add(querySegmentTree(root, 0, num - 1));  // range: [0, query - 1]
    }
    return result;
  }

  private SegmentTreeNode buildSegmentTree(int start, int end) {
    if (start > end) {
      return null;
    } else if (start == end) {
      return new SegmentTreeNode(start, end);
    }
    SegmentTreeNode root = new SegmentTreeNode(start, end);
    int mid = start + (end - start) / 2;
    root.left = buildSegmentTree(start, mid);
    root.right = buildSegmentTree(mid + 1, end);
    return root;
  }

  private void modifySegmentTree(SegmentTreeNode root, int num, int count) {
    if (root == null || num > root.end || num < root.start) {
      return ;
    } 
    if (root.start == num && root.end == num ) {
      root.count += count;
      return;
    }
    modifySegmentTree(root.left, num, count);
    modifySegmentTree(root.right, num, count);
    int leftCount = root.left == null ? 0 : root.left.count;
    int rightCount = root.right == null ? 0 : root.right.count;
    root.count = leftCount + rightCount;
  }

  private int querySegmentTree(SegmentTreeNode root, int start, int end) {
    if (root == null || start > root.end || end < root.start) {
      return 0;
    }
    if (root.start >= start && root.end <= end) {
      return root.count;
    }
    return querySegmentTree(root.left, start, end) + querySegmentTree(root.right, start, end);
  }

  class SegmentTreeNode {
    int start;
    int end;
    int count;  // default value 0
    SegmentTreeNode left;
    SegmentTreeNode right;

    public SegmentTreeNode(int start, int end) {
      this.start = start;
      this.end = end;
    }
  }
}
```

