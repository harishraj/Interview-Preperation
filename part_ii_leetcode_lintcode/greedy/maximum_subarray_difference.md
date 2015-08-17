# Maximum Subarray Difference
> Given an array with integers.
>
> Find two non-overlapping subarrays A and B, which |SUM(A) - SUM(B)| is the largest.
>
> Return the largest difference.
>
> Example
>
> For [1, 2, -3, 1], return 6

## Method 1: Greedy
### Analysis
For each element in the array, get the max and min value in the left and right side. Then it is easy to find a split point that maximize the difference. 

### Related Question
1. [Maximum Subarray II](maximum_subarray_ii.md)

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public int maxDiffSubArrays(ArrayList<Integer> nums) {
    int len = nums.size();
    int[] leftMax = new int[len];
    int[] leftMin = new int[len];
    int curMaxSum = 0;
    int curMinSum = 0;
    // scan from left to right
    for (int i = 0; i < len; i++) {
      curMaxSum = Math.max(curMaxSum + nums.get(i), nums.get(i));
      curMinSum = Math.min(curMinSum + nums.get(i), nums.get(i));
      if (i == 0) {
        leftMax[i] = curMaxSum;
        leftMin[i] = curMinSum;
      } else {
        leftMax[i] = Math.max(leftMax[i - 1], curMaxSum);
        leftMin[i] = Math.min(leftMin[i - 1], curMinSum);
      }
    }
    // scan from right to left
    curMaxSum = 0;
    curMinSum = 0;
    int rightMax = 0;
    int rightMin = 0;
    int maxDiff = 0;
    for (int i = len - 1; i > 0; i--) {
      curMaxSum = Math.max(curMaxSum + nums.get(i), nums.get(i));
      curMinSum = Math.min(curMinSum + nums.get(i), nums.get(i));
      if (i == len - 1) {
        rightMax = curMaxSum;
        rightMin = curMinSum;
      } else {
        rightMax = Math.max(rightMax, curMaxSum);
        rightMin = Math.min(rightMin, curMinSum);
      }
      maxDiff = Math.max(
          maxDiff, 
          Math.max(
              Math.abs(leftMax[i - 1] - rightMin), 
              Math.abs(rightMax - leftMin[i - 1])));
    }
    return maxDiff;
  }
}
```
