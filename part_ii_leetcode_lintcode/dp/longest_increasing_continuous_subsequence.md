# Longest Increasing Continuous subsequence
> Give you an integer array (index from 0 to n-1, where n is the size of this array)，find the longest increasing continuous subsequence in this array. (The definition of the longest increasing continuous subsequence here can be from right to left or from left to right)
>
> Example
>
> For [5, 4, 2, 1, 3], the LICS is [5, 4, 2, 1], return 4.
>
> For [5, 1, 2, 3, 4], the LICS is [1, 2, 3, 4], return 4.

### Analysis
One method is to scan the array twice, one from the left and one from the right to keep the longest increasing subsequence from two directions and return the bigger one. However, we can implement this only using one scan of the array by combining this two process to find the longest increasing and decreasing subsequence. 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int longestIncreasingContinuousSubsequence(int[] A) {
    if (A == null || A.length == 0) {
      return 0;
    }
    int maxIncreasingLen = 1;
    int maxDecreasingLen = 1;
    int curIncreasinglen = 1;
    int curDecreasinglen = 1;
    // scan from left to right
    for (int i = 1; i < A.length; i++) {
      if (A[i] > A[i - 1]) {
        maxIncreasingLen = Math.max(maxIncreasingLen, ++curIncreasinglen);
        curDecreasinglen = 1;
      } else if (A[i] < A[i - 1]) {
        maxDecreasingLen = Math.max(maxDecreasingLen, ++curDecreasinglen);
        curIncreasinglen = 1;
      } else {
        maxIncreasingLen = Math.max(maxIncreasingLen, ++curIncreasinglen);
        maxDecreasingLen = Math.max(maxDecreasingLen, ++curDecreasinglen);
      }
    }
    return Math.max(maxIncreasingLen, maxDecreasingLen);
  }
}
```
