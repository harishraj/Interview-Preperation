# Interval Sum
> Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers [start, end]. For each query, calculate the sum number between index start and end in the given array, return the result list.
>
> Example
>
> For array [1,2,7,8,5], and queries [(0,4),(1,2),(2,4)], return [23,9,20]

### Analysis
Since it needs to query intervals, it's better to use segment tree.

### Complexity
Time: O(n) to build tree and O(lgn) for each query.

Space: O(n)

### Code
#### Java
```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */
public class Solution {
  /**
   *@param A, queries: Given an integer array and an query list
   *@return: The result list
   */
  public ArrayList<Long> intervalSum(int[] A, ArrayList<Interval> queries) {
    ArrayList<Long> result = new ArrayList<>();
    // build segment tree
    SegmentTreeNode root = buildSegmentTree(A, 0, A.length - 1);
    // query segment tree
    for (Interval interval : queries) {
      result.add(querySegmentTree(root, interval.start, interval.end));
    }
    return result;
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

  private long querySegmentTree(SegmentTreeNode root, int start, int end) {
    if (root == null || root.start > end || root.end < start) {
      return 0;
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
