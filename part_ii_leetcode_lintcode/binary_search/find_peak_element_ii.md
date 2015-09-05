# Find Peak Element II
### Source
1. [lintcode](http://www.lintcode.com/en/problem/find-peak-element-ii/)

> There is an integer matrix which has the following features:
> 
> * The numbers in adjacent positions are different.
> * The matrix has n rows and m columns.
> * For all i < m, A[0][i] < A[1][i] && A[n - 2][i] > A[n - 1][i].
> * For all j < n, A[j][0] < A[j][1] && A[j][m - 2] > A[j][m - 1].
>
> We define a position P is a peek if:
>
> A[j][i] > A[j+1][i] && A[j][i] > A[j-1][i] && A[j][i] > A[j][i+1] && A[j][i] > A[j][i-1]
>
> Find a peak element in this matrix. Return the index of the peak.
>
> Example
>
> Given a matrix:
>
>     [
>       [1 ,2 ,3 ,6 ,5],
>       [16,41,23,22,6],
>       [15,17,24,21,7],
>       [14,18,19,20,10],
>       [13,14,11,10,9]
>     ]
>
> return index of 41 (which is [1,1]) or index of 24 (which is [2,2])

### Analysis
Do binary search on each row as [Find Peak Element](find_peak_element.md).

### Complexity
Time: O(m * logn)

Space: O(1)

### Code
#### Java
```java
class Solution {
  public List<Integer> findPeakII(int[][] A) {
    List<Integer> result = new ArrayList<>();
    for (int row = 0; row < A.length; row++) {
      int col = findPeakInRow(A[row]);
      if ((row == 0 || A[row][col] > A[row - 1][col]) && 
          (row == A.length - 1 || A[row][col] > A[row + 1][col])) {
        result.add(row);
        result.add(col);
        break;
      }
    }
    return result;
  }

  private int findPeakInRow(int[] A) {
    int start = 0;
    int end = A.length - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;
      if (A[mid] > A[mid + 1]) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }
    return start;
  }
}
```
