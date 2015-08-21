# Interval Minimum Number
> Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers [start, end]. For each query, calculate the minimum number between index start and end in the given array, return the result list.
>
> Example
>
> For array [1,2,7,8,5], and queries [(1,2),(0,4),(2,4)], return [2,1,5]

### Complexity
Time: O(lgn) for each query

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
  public ArrayList<Integer> intervalMinNumber(int[] A, ArrayList<Interval> queries) {
    ArrayList<Integer> result = new ArrayList<>();
    SegmentTreeNode root = buildSegmentTree(A, 0, A.length - 1);
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
    SegmentTreeNode root = new SegmentTreeNode(start, end, 0);
    int mid = start + (end - start) / 2;
    root.left = buildSegmentTree(A, start, mid);
    root.right = buildSegmentTree(A, mid + 1, end);
    int leftMin = root.left == null ? Integer.MAX_VALUE : root.left.minValue;
    int rightMin = root.right == null ? Integer.MAX_VALUE : root.right.minValue;
    root.minValue = Math.min(leftMin, rightMin);
    return root;
  }

  private int querySegmentTree(SegmentTreeNode root, int start, int end) {
    if (root == null || root.start > end || root.end < start) {
      return Integer.MAX_VALUE;
    }
    if (root.start >= start && root.end <= end) {
      return root.minValue;
    }
    return Math.min(querySegmentTree(root.left, start, end), querySegmentTree(root.right, start, end));
  }

  class SegmentTreeNode {
    int start;
    int end;
    int minValue;  // default value 0L
    SegmentTreeNode left;
    SegmentTreeNode right;

    public SegmentTreeNode(int start, int end, int minValue) {
      this.start = start;
      this.end = end;
      this.minValue = minValue;
    }
  }
}
```
