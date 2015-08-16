# Kth Smallest Number in Sorted Matrix
> Find the kth smallest number in at row and column sorted matrix.
>
> Example
>
> Given k = 4 and a matrix:
>
>     [
>       [1 ,5 ,7],
>       [3 ,7 ,8],
>       [4 ,8 ,9],
>     ]
>
> return 5

## Method 1: Heap
### Analysis
We can push the first elements of each row into a heap, and then every time we poll the smallest number and put its successor to the heap again until we find the kth smallest number.

The idea is actually the same as merge k sorted lists using heap. 

### Complexity
Time: O(klg(min(m, n))), where m is the number of rows and n is the number of cols.

Space: O(min(m, n))

### Code
#### Java
```java
public class Solution {
  public int kthSmallest(int[][] matrix, int k) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    if (k <= 0 || k > matrix.length * matrix[0].length) {
      return 0;
    }

    int num;
    if (matrix.length < matrix[0].length) {
      num = searchHorizontal(matrix, k);
    } else {
      num = searchVertical(matrix, k);
    }
    return num;
  }

  private int searchHorizontal(int[][] matrix, int k) {
    PriorityQueue<Cell> pq = new PriorityQueue<>();
    for (int i = 0; i < matrix.length; i++) {
      pq.offer(new Cell(i, 0, matrix[i][0]));
    }
    Cell cell = null;
    for (int i = 0; i < k; i++) {
      cell = pq.poll();
      if (cell.y + 1 < matrix[0].length) {
        pq.offer(new Cell(cell.x, cell.y + 1, matrix[cell.x][cell.y + 1]));
      }
    }
    return cell.val;
  }

  private int searchVertical(int[][] matrix, int k) {
    PriorityQueue<Cell> pq = new PriorityQueue<>();
    for (int j = 0; j < matrix[0].length; j++) {
      pq.offer(new Cell(0, j, matrix[0][j]));
    }
    Cell cell = null;
    for (int i = 0; i < k; i++) {
      cell = pq.poll();
      if (cell.x + 1 < matrix.length) {
        pq.offer(new Cell(cell.x + 1, cell.y, matrix[cell.x + 1][cell.y]));
      }
    }
    return cell.val;
  }

  class Cell implements Comparable<Cell> {
    int x;
    int y;
    int val;
    
    public Cell(int x, int y, int val) {
      this.x = x;
      this.y = y;
      this.val = val;
    }

    @Override
    public int compareTo(Cell that) {
      return Integer.compare(this.val, that.val);
    }
  }
}
```
