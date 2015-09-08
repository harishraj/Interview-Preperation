# Submatrix Sum
1. [lintcode](http://www.lintcode.com/en/problem/submatrix-sum/)

> Given an integer matrix, find a submatrix where the sum of numbers is zero. Your code should return the coordinate of the left-up and right-down number.
>
> Example
>
> Given matrix
>
>     [
>       [1 ,5 ,7],
>       [3 ,7 ,-8],
>       [4 ,-8 ,9],
>     ]
>
> return [(1,1), (2,2)]

### Analysis
The same idea as [Subarray Sum](subarray_sum.md).

### Complexity
Time: O(m^2 * n)

Space: O(m*n)

### Code
```java
public class Solution {
  public int[][] submatrixSum(int[][] matrix) {
    assert(matrix != null && matrix.length != 0 && matrix[0].length != 0);
    int numRows = matrix.length;
    int numCols = matrix[0].length;
    int[][] sum = new int[numRows + 1][numCols + 1];
    // preprocess to get sum of all submatrix [(0, 0), (i, j)]
    for (int i = 0; i < numRows; i++) {
      for (int j = 0; j < numCols; j++) {
        sum[i + 1][j + 1] = matrix[i][j] + sum[i + 1][j] + sum[i][j + 1] - sum[i][j];
      }
    }

    Map<Integer, Integer> map = new HashMap<>();
    for (int i1 = 0; i1 < numRows; i1++) {
      for (int i2 = i1 + 1; i2 <= numRows; i2++) {
        map.clear();
        map.put(0, 0);
        for (int j = 1; j <= numCols; j++) {
          int diff = sum[i2][j] - sum[i1][j];
          if (map.containsKey(diff)) {
            return new int[][]{{i1, map.get(diff)}, {i2 - 1, j - 1}};
          }
          map.put(diff, j);
        }
      }
    }
    return null;
  }
}
```


