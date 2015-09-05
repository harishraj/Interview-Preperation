# N-Queens II
### Source
1. [leetcode](https://leetcode.com/problems/n-queens-ii/)
2. [lintcode](http://www.lintcode.com/en/problem/n-queens-ii/)

> Follow up for [N-Queens](n_queens.md) problem.
>
> Now, instead outputting board configurations, return the total number of distinct solutions.

## Method 1: O(1) check conflict
### Code
#### Java
```java
public class Solution {
  private boolean[] colConflict;
  private boolean[] diagConflict;
  private boolean[] revDiagConflict;
  private int numSolutions;

  public int totalNQueens(int n) {
    colConflict = new boolean[n];
    diagConflict = new boolean[2 * n];
    revDiagConflict = new boolean[2 * n];
    numSolutions = 0;
    dfs(n, new int[n], 0);
    return numSolutions;
  }

  private void dfs(int n, int[] colPos, int row) {
    if (row == n) {
      numSolutions++;
      return;
    }
    for (int col = 0; col < n; col++) {
      if (isConflict(n, row, col)) {
        continue;
      }
      setConflict(n, row, col, true);
      colPos[row] = col;
      dfs(n, colPos, row + 1);
      setConflict(n, row, col, false);
    }
  }

  private boolean isConflict(int n, int row, int col) {
    if (colConflict[col] || diagConflict[row - col + n] || 
        revDiagConflict[row + col]) {
      return true;
    }
    return false;
  }

  private void setConflict(int n, int row, int col, boolean conflict) {
    colConflict[col] = conflict;
    diagConflict[row - col + n] = conflict;
    revDiagConflict[row + col] = conflict;
  }
}
```

## Method 2: Symmetric Property
### Code
#### Java
```java
public class Solution {
  private boolean[] colConflict;
  private boolean[] diagConflict;
  private boolean[] revDiagConflict;
  private int numSolutions;
  private int numMidColSolutins;
  private int secondHalfIndex;

  public int totalNQueens(int n) {
    colConflict = new boolean[n];
    diagConflict = new boolean[2 * n];
    revDiagConflict = new boolean[2 * n];
    numSolutions = 0;
    numMidColSolutins = 0;
    secondHalfIndex = (n - 1) / 2 + 1;
    dfs(n, new int[n], 0);
    return numSolutions * 2 + numMidColSolutins;
  }

  private void dfs(int n, int[] colPos, int row) {
    if (row == n) {
      if (colPos[0] != n / 2) {
        numSolutions++;
      } else {
        numMidColSolutins++;
      }
      return;
    }
    for (int col = 0; col < n; col++) {
      if (row == 0 && col == secondHalfIndex) {
        break;
      }
      if (isConflict(n, row, col)) {
        continue;
      }
      setConflict(n, row, col, true);
      colPos[row] = col;
      dfs(n, colPos, row + 1);
      setConflict(n, row, col, false);
    }
  }

  private boolean isConflict(int n, int row, int col) {
    if (colConflict[col] || diagConflict[row - col + n] || 
        revDiagConflict[row + col]) {
      return true;
    }
    return false;
  }

  private void setConflict(int n, int row, int col, boolean conflict) {
    colConflict[col] = conflict;
    diagConflict[row - col + n] = conflict;
    revDiagConflict[row + col] = conflict;
  }
}
```

## Method 3: Bit Manipulation
### Code
#### Java
```java
public class Solution {
  private int upperLimit;
  private int result;
  
  public int totalNQueens(int n) {
    // check paramter
    if (n <= 0) {
      return 0;
    }
    upperLimit = (1 << n) - 1;
    result = 0;
    dfs(0, 0, 0);
    return result;
  }
  
  private void dfs(int col, int ld, int rd) {
    if (col == upperLimit) {  // all queen has been placed
      result++;
      return;  
    }
    int possiblePosition = upperLimit & ~(col | ld | rd);
    while (possiblePosition != 0) {
      // get the rightmost possible bit
      int pos = possiblePosition & (-possiblePosition);
      possiblePosition -= pos; // update possiblePosition
      
      dfs(col + pos, (ld + pos) << 1, (rd + pos) >> 1);
    }
  }
}
```
