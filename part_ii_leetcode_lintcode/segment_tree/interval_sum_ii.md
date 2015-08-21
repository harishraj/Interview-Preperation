# Interval Sum II
> Given an integer array in the construct method, implement two methods query(start, end) and modify(index, value):
>
> For query(start, end), return the sum from index start to index end in the given array.
>
> For modify(index, value), modify the number in the given index to value
>
> Example
>
> Given array A = [1,2,7,8,5].
>
> * query(0, 2), return 10.
> * modify(0, 4), change A[0] from 1 to 4.
> * query(0, 1), return 6.
> * modify(2, 1), change A[2] from 7 to 1.
> * query(2, 4), return 14.

### Analysis
Almost the same as [Interval Sum](interval_sum.md).

### Complexity
Time: O(lgn) for both modify and query

Space: O(n)

### Code
#### Java
```java
public class Solution {
  SegmentTreeNode root;
  public Solution(int[] A) {
    root = buildSegmentTree(A, 0, A.length - 1);
  }
  
  /**
   * @param start, end: Indices
   * @return: The sum from start to end
   */
  public long query(int start, int end) {
    return querySegmentTree(root, start, end);
  }
  
  /**
   * @param index, value: modify A[index] to value.
   */
  public void modify(int index, int value) {
    modifySegmentTree(root, index, value);
  }

  private SegmentTreeNode buildSegmentTree(int[] A, int start, int end) {
    if (start > end) {
      return null;
    } else if (start == end) {
      return new SegmentTreeNode(start, end, A[start]);
    }
    SegmentTreeNode root = new SegmentTreeNode(start, end, 0L);
    int mid = start + (end - start) / 2;
    root.left = buildSegmentTree(A, start, mid);
    root.right = buildSegmentTree(A, mid + 1, end);
    long leftSum = root.left == null ? 0L : root.left.sum;
    long rightSum = root.right == null ? 0L : root.right.sum;
    root.sum = leftSum + rightSum;
    return root;
  }

  private void modifySegmentTree(SegmentTreeNode root, int index, int value) {
    if (root == null || index < root.start || index > root.end) {
      return;
    }
    if (root.start == index && root.end == index) {
      root.sum = value;
      return;
    }
    modifySegmentTree(root.left, index, value);
    modifySegmentTree(root.right, index, value);
    long leftSum = root.left == null ? 0L : root.left.sum;
    long rightSum = root.right == null ? 0L : root.right.sum;
    root.sum = leftSum + rightSum;
    return;
  }

  private long querySegmentTree(SegmentTreeNode root, int start, int end) {
    if (root == null || root.start > end || root.end < start) {
      return 0L;
    }
    if (root.start >= start && root.end <= end) {
      return root.sum;
    }
    return querySegmentTree(root.left, start, end) + querySegmentTree(root.right, start, end);
  }

  class SegmentTreeNode {
    int start;
    int end;
    long sum;  // default value 0L
    SegmentTreeNode left;
    SegmentTreeNode right;

    public SegmentTreeNode(int start, int end, long sum) {
      this.start = start;
      this.end = end;
      this.sum = sum;
    }
  }
}
```
