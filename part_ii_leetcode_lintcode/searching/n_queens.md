# N-Queens
### Source
1. [leetcode](https://leetcode.com/problems/n-queens/)
2. [lintcode](http://www.lintcode.com/en/problem/n-queens/)

> The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.
>
> ![image](http://www.leetcode.com/wp-content/uploads/2012/03/8-queens.png)
>
> Given an integer n, return all distinct solutions to the n-queens puzzle.
>
> Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.
>
> For example,
>
> There exist two distinct solutions to the 4-queens puzzle:
>
>   [
>   [".Q..",  // Solution 1
>    "...Q",
>      "Q...",
>      "..Q."],
>
>     ["..Q.",  // Solution 2
>      "Q...",
>      "...Q",
>      ".Q.."]
>   ]

## Method 1: O(1) check conflict
### Analysis
Since we need to get all answers, we need to search the whole space(although we can prune). It is easier to code using DFS. 

The problem here is we need to find a way to record the checkerboard's state, and judge if a new queue position will attack with any existing queues. 

The most naive way is to use a matrix to represent the whole checkerboard. 

Since the checkerboard is very sparse, and there will not be any two queues on the same row/col. we can simple use to 1-d array to record existing queue's row/col position. Then how can we judge conflicting? 

#### O(N) time check conflict
1. The new queue is on the same row/col as any existing queue. (Actually cannot be in the same row as we are searching from row 1 to row n)
2. The new queue is on the left diagonal of an existing queue. It looks like this '\'(Haha!). We can judge this like `new_row - existing_row == new_col - existing_col`
3. The new queue is on the right diagonal('\') of an existing queue. We can judge this using: `new_row - existing_row == existing_col - new_col`

#### O(1) time check conflict
However, we can do this in O(1) with extra space. Instead of simply keeping colFlag, we use two extra array: leftDiag and rightDiag. 

Consider the following cases:

1. left diagonal('\'), `new_row - existing_row == new_col - existing_col` can be written to `new_row - new_col == existring_row - existing_col`. So we only need to mark if **row - col** has appeared before. To deal with the problem that **row - col < 0**, we use **row - col + n** as array's index.
2. right diagonal('/'), `new_row - existing_row == existing_col - new_col` can be written to `new_row + new_cor == existing_row + existing_col`. We can use **row + col** to show if there is a conflict. 

### Comlexity
Time: O(n^n)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  private List<List<String>> result;
  private boolean[] colConflict;
  private boolean[] diagConflict;
  private boolean[] revDiagConflict;
  private char[] initState;

  public List<List<String>> solveNQueens(int n) {
    result = new ArrayList<>();
    colConflict = new boolean[n];
    diagConflict = new boolean[2 * n];
    revDiagConflict = new boolean[2 * n];
    initState = new char[n];
    Arrays.fill(initState, '.');
    dfs(n, new int[n], 0);
    return result;
  }

  private void dfs(int n, int[] colPos, int row) {
    if (row == n) {
      result.add(generateResult(n, colPos));
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

  private List<String> generateResult(int n, int[] colPos) {
    List<String> solution = new ArrayList();
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < n; i++) {
      initState[colPos[i]] = 'Q';
      solution.add(new String(initState));
      initState[colPos[i]] = '.';
    }
    return solution;
  }
}
```

## Method 2: Symmetric
### Analysis
We can do even better!

This problem is symmetry, take the 4X4 checkerboard as an example. When we place the first queen in the first row, we only need to consider the case that we put it in the 1st and 2nd column. The remaining case can be mirrored using previous solution. We simply mirror each row in solution 1. 

    .Q.. -> ..Q.
    ...Q -> Q...
    Q... -> ...Q
    ..Q. -> .Q..

**Notice** that we do not need to reverse the middle column if n is odd. 

### Complexity
Time: O(n^(n/2))

Space: O(n)

### Code
#### Java
```java
public class Solution {
  private List<List<String>> result;
  private boolean[] colConflict;
  private boolean[] diagConflict;
  private boolean[] revDiagConflict;
  private char[] initState;
  private int firstHalfResultNum;
  private int secondHalfIndex;

  public List<List<String>> solveNQueens(int n) {
    result = new ArrayList<>();
    colConflict = new boolean[n];
    diagConflict = new boolean[2 * n];
    revDiagConflict = new boolean[2 * n];
    initState = new char[n];
    Arrays.fill(initState, '.');
    firstHalfResultNum = 0;
    secondHalfIndex = (n - 1) / 2 + 1;
    dfs(n, new int[n], 0);
    generateSymResult();
    return result;
  }

  private void dfs(int n, int[] colPos, int row) {
    if (row == n) {
      if (colPos[0] != n / 2) {
        firstHalfResultNum++;
      }
      result.add(generateResult(n, colPos));
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

  private List<String> generateResult(int n, int[] colPos) {
    List<String> solution = new ArrayList();
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < n; i++) {
      initState[colPos[i]] = 'Q';
      solution.add(new String(initState));
      initState[colPos[i]] = '.';
    }
    return solution;
  }

  private void generateSymResult() {
    for (int i = 0; i < firstHalfResultNum; i++) {
      List<String> symResult = new ArrayList<>();
      for (String str : result.get(i)) {
        symResult.add(new StringBuilder(str).reverse().toString());
      }
      result.add(symResult);
    }
  }
}
```

