# The Smallest Difference
> Given two array of integers(the first array is array A, the second array is array B), now we are going to find a element in array A which is A[i], and another element in array B which is B[j], so that the difference between A[i] and B[j] (|A[i] - B[j]|) is as small as possible, return their smallest difference.
>
> Example
>
> For example, given array A = [3,6,7,4], B = [2,8,9,3], return 0

## Method 1: Two Pointers
### Analysis
First sort two arrays and use two pointers to sovle the problem. Each time advance the pointer to smaller number by 1. 

### Complexity
Time: O(NlgN)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int smallestDifference(int[] A, int[] B) {
    Arrays.sort(A);
    Arrays.sort(B);
    int index1 = 0;
    int index2 = 0;
    int minDiff = Integer.MAX_VALUE;
    while (index1 < A.length && index2 < B.length) {
      minDiff = Math.min(minDiff, Math.abs(A[index1] - B[index2]));
      if (minDiff == 0) {
        break;
      }
      if (A[index1] < B[index2]) {
        index1++;
      } else {
        index2++;
      }
    }
    return minDiff;
  }
}
```
