# K Sum
### Source
1. [lintcode](http://www.lintcode.com/en/problem/k-sum/)

> Given n distinct positive integers, integer k (k <= n) and a number target.
>
> Find k numbers where sum is target. Calculate how many solutions there are?
>
> Example
>
> Given [1,2,3,4], k = 2, target = 5.
>
> There are 2 solutions: [1,4] and [2,3].
>
> Return 2.

## Method 1: DFS (Got TLE)
### Analysis
Simply choose k numbers from the array and check if they sum up to target.

### Complexity
Time: O(C(n, k))

Space: O(k)

### Code
#### Java
```java
public class Solution {
  /**
   * @param A: an integer array.
   * @param k: a positive integer (k <= length(A))
   * @param target: a integer
   * @return an integer
   */
  public int kSum(int A[], int k, int target) {
    if (k < 1 || target <= 0) {
      return 0;
    }
    return kSumHelper(A, k, target, 0);
  }

  private int kSumHelper(int A[], int k, int target, int start) {
    if (k == 0) {
      return target == 0 ? 1 : 0;
    }
    int count = 0;
    for (int i = start ; i < A.length; i++) {
      if (A[i] > target) {
        continue;
      }
      count += kSumHelper(A, k - 1, target - A[i], i + 1);
    }
    return count;
  }
}
```

## Method 2: DP
### Analysis
count[i][j][v] means choosing i numbers from the first j numbers, which sum up to v. We can update it by:

1. `count[i][j][v] += count[i][j - 1][v]` which means not choosing the new number.
2. `count[i][j][v] += count[i - 1][j - 1][v - nums[j]]` which means choosing the new number.

### Complexity
Time: O(len * k * target)

Space: O(len * k * target)

### Code
#### Java
```java
public class Solution {
  /**
   * @param A: an integer array.
   * @param k: a positive integer (k <= length(A))
   * @param target: a integer
   * @return an integer
   */
  public int kSum(int A[], int k, int target) {
    if (k < 1 || target <= 0) {
      return 0;
    }
    int len = A.length;
    int count[][][] = new int[k + 1][len + 1][target + 1];
    for (int i = 1; i <= len; i++) {
      if (A[i - 1] <= target) {
        for (int j = i; j <= len; j++) {
          count[1][j][A[i - 1]]++;
        }
      }
    }
    for (int i = 2; i <= k; i++) {
      for (int j = i; j <= len; j++) {
        for (int v = 1; v <= target; v++) {
          count[i][j][v] += count[i][j - 1][v];
          if (v - A[j - 1] >= 0) {
            count[i][j][v] += count[i - 1][j - 1][v - A[j - 1]]; 
          }
        }
      }
    }

    return count[k][len][target];
  }
}
```
