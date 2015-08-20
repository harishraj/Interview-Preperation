# Count of Smaller Number
> Give you an integer array (index from 0 to n-1, where n is the size of this array, value from 0 to 10000) and an query list. For each query, give you an integer, return the number of element in the array that are smaller that the given integer.
>
> Example
>
> For array [1,2,7,8,5], and queries [1,8,5], return [0,4,2]

## Method 1: Loop
### Analysis
Simply loop the array to find number of elements smaller than it. 

### Complexity
Time: O(k*n), k is the number of elements to query, n is the number of elements in the array.

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
    ArrayList<Integer> result = new ArrayList<>();
    for (int query : queries) {
      int count = 0;
      for (int num : A) {
        if (num < query) {
          ++count;
        }
      }
      result.add(count);
    }
    return result;
  }
}
```

## Method 2: Binary Search
### Analysis
If the array is very large and there are many elements to query, it is better to sort the arrays first, and each query only needs to take O(lgn) time instead of o(n) time.

Note that the binary search should also take into consideration of duplicate value.

### Complexity
Time: O(nlgn) for preprocess, O(klgn) for query.

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
    ArrayList<Integer> result = new ArrayList<>();
    Arrays.sort(A);
    for (int query : queries) {
      result.add(binarySearch(A, query));
    }
    return result;
  }

  /**
   * Return how many elements is smaller than target
   */
  private int binarySearch(int[] A, int target) {
    int start = 0;
    int end = A.length - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (A[mid] == target) {
        if (mid == 0 || A[mid - 1] != target) {
          return mid;
        } else {
          end = mid - 1;
        }
      } else if (A[mid] < target) {
        start = mid + 1;
      } else {
        end = mid - 1;
      }
    }
    return end + 1;
  }
}
```

## Method 3: Segment Tree
### Analysis
Build a segment tree to do the query.

### Complexity
Time: O(n) to build the tree, O(nlgn) to update counts in the tree, O(lgn) for query

Space: O(n)

### Code
#### Java
```java
public class Solution {
  public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
    ArrayList<Integer> result = new ArrayList<>();
    // preprocess
    SegmentTreeNode root = buildSegmentTree(0, 10000);
    for (int num : A) {
      modifySegmentTree(root, num, 1);
    }
    // query segment tree to get result
    for (int query : queries) {
      result.add(querySegmentTree(root, 0, query - 1));  // range: [0, query - 1]
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
