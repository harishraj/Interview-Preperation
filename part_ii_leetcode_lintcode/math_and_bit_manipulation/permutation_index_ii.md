# Permutation Index II
> Given a permutation which may contain repeated numbers, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.
>
> Example
>
> Given the permutation [1, 4, 2, 2], return 3.

### Analysis
The difference between [Permutation Index](permutation_index.md) is that we need to figure out the number of permutations before it when taking duplication into consideration. The number of permutations with duplicates are n!/(k1!*k2!*k3!...), where k1, k2, k3... are number of duplicates for each number.

For example, [2, 2, 1, 1]. For the first 2, since there are two smaller numbers, the permutations before it is 2 * (3!/(2!*2!)) = 3. For the second 2, since there are two smaller numbers, the permutations before it is 2 * (2!/2!) = 2. So the final index is 1 + 3 + 2 = 6.

How can we derive the formula? For example we have n numbers, and there are three distinct numbers. So the total number of permutations are `n! / (k1! * k2! * k3!)`. For each number, it owns 1/n of the permutations, so it is the total number divided by n, which is `(n-1)! / (k1! * k2! * k3!)`. If there are m numbers smaller than the current number, then there must be `m * (n-1)! / (k1! * k2! * k3!)` permutations before it.

### Complexity
Time: O(n^2)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  /**
   * @param A an integer array
   * @return a long integer
   */
  public long permutationIndexII(int[] A) {
    long index = 1;
    long factor = getFactor(A.length);
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < A.length; i++) {
      factor /= A.length - i;
      map.clear();
      map.put(A[i], 1);  // important to include itself
      long rank = 0;
      for (int j = i + 1; j < A.length; j++) {
        if (map.containsKey(A[j])) {
          map.put(A[j], map.get(A[j]) + 1);
        } else {
          map.put(A[j], 1);
        }
        if (A[i] > A[j]) {
          rank++;
        }
      }
      index += rank * factor / getDupFactor(map);
    }
    return index;
  }

  private long getDupFactor(Map<Integer, Integer> map) {
    long dup = 1;
    for (int val : map.values()) {
      dup *= getFactor(val);
    }
    return dup;
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
