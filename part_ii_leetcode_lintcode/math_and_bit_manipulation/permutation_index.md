# Permutation Index
> Given a permutation which contains no repeated number, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.
>
> Example
>
> Given [1,2,4], return 1.

### Analysis
Consider the case [4, 2, 1], we found that there are two numbers smaller than 4, so there must be at least 2 * 2! = 4 permutations in front of [4, 2, 1], which is [1, 2, 4], [1, 4, 2], [2, 1, 4] and [2, 4, 1]. Consider the second number 2, since there is one number smaller than it, there must be at least 1 * 1! = 1 permutations in front of it, which is [4, 1, 2]. So we can get the final index 1 + 2 * 2! + 1 * 1! = 6.

### Complexity
Time: O(n^2)

Space: O(1)

### Code
#### Java Version 1: Scan from front to end
```java
public class Solution {
  /**
   * @param A an integer array
   * @return a long integer
   */
  public long permutationIndex(int[] A) {
    if (A == null || A.length == 0) {
      return 0;
    }
    long index = 1;
    long factor = getFactor(A.length);
    for (int i = 0; i < A.length; i++) {
      factor /= (A.length - i);
      int rank = 0;
      for (int j = i + 1; j < A.length; j++) {
        if (A[i] > A[j]) {
          rank++;
        }
      }
      index += factor * rank;
    }
    return index;
  }

  private long getFactor(int num) {
    long factor = 1;
    for (int i = 2; i <= num; i++) {
      factor *= i;
    }
    return factor;
  }
}
```

#### Java Version 2: Scan from end to front
```java
public class Solution {
  /**
   * @param A an integer array
   * @return a long integer
   */
  public long permutationIndex(int[] A) {
    if (A == null || A.length == 0) {
      return 0;
    }
    long index = 1;
    long factor = 1;
    for (int i = A.length - 1; i >= 0; i--) {
      int rank = 0;
      for (int j = i + 1; j < A.length; j++) {
        if (A[i] > A[j]) {
          rank++;
        }
      }
      index += factor * rank;
      factor *= (A.length - i);
    }
    return index;
  }
}
```

