# K Sum II
### Source
1. [lintcode](http://www.lintcode.com/en/problem/k-sum-ii/)

> Given n unique integers, number k (1<=k<=n)  and target. Find all possible k integers where their sum is target.
>
> Example
>
> Given [1,2,3,4], k=2, target=5, [1,4] and [2,3] are possible solutions.

## Method 1: DFS
### Analysis
Simply do DFS.

### Complexity
Time: O(n^k)

Space: O(k)

### Code
#### Java
```java
public class Solution {
  private ArrayList<ArrayList<Integer>> result;

  public ArrayList<ArrayList<Integer>> kSumII(int A[], int k, int target) {
     result = new ArrayList<>();
     dfs(A, k, target, new int[A.length], 0, 0);
     return result;
  }

  private void dfs(int[] A, int k, int target, int[] path, int index, int start) {
    if (index == k) {
      if (target == 0) {
        ArrayList<Integer> newResult = new ArrayList<>();
        for (int i = 0; i < k; i++) {
          newResult.add(path[i]);
        }
        result.add(newResult);
      }
      return;
    }
    for (int i = start; i < A.length; i++) {
      path[index] = A[i];
      dfs(A, k, target - A[i], path, index + 1, i + 1);
    }
  }
}
```

