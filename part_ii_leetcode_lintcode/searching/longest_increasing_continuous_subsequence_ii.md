# Longest Increasing Continuous subsequence II
> Give you an integer matrix (with row size n, column size m)，find the longest increasing continuous subsequence in this matrix. (The definition of the longest increasing continuous subsequence here can start at any row or column and go up/down/right/left any direction).
> 
> Example
>
> Given a matrix:
> 
>     [
>       [1 ,2 ,3 ,4 ,5],
>       [16,17,24,23,6],
>       [15,18,25,22,7],
>       [14,19,20,21,8],
>       [13,12,11,10,9]
>     ]
>
> return 25

## Method 1: DFS + Memo
### Analysis
We can start from each cell and keep searching the longest continuous subsequence. Since there are lots of duplicate search, we can use a cache to record the maximum length that can be found starting from a cell.

### Complexity
Time: O(mn)

Space: O(mn)

### Code
#### Java
```java
public class Solution {
  private static final int[][] DIRS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
  private int[][] cache;

  public int longestIncreasingContinuousSubsequenceII(int[][] A) {
    if (A == null || A.length == 0 || A[0].length == 0) {
      return 0;
    }
    int maxLen = 0;
    int numRows = A.length;
    int numCols = A[0].length;
    cache = new int[numRows][numCols];
    for (int i = 0; i < numRows; i++) {
      for (int j = 0; j < numCols; j++) {
        maxLen = Math.max(maxLen, dfs(A, i, j, 1, new boolean[numRows][numCols]));
      }
    }
    return maxLen;
  }

  private int dfs(int[][] A, int x, int y, int len, boolean[][] visited) {
    if (visited[x][y]) {
      return len;
    } else if (cache[x][y] != 0) {
      return cache[x][y];
    }
    visited[x][y] = true;
    int maxLen = 0;
    for (int i = 0; i < DIRS.length; i++) {
      int newX = x + DIRS[i][0];
      int newY = y + DIRS[i][1];
      if (newX >= 0 && newX < A.length && newY >= 0 && newY < A[0].length 
          && A[newX][newY] > A[x][y]) {
        maxLen = Math.max(maxLen, dfs(A, newX, newY, len + 1, visited));
      }
    }
    visited[x][y] = false;
    cache[x][y] = maxLen + 1;
    return maxLen + 1;
  }
}
```
