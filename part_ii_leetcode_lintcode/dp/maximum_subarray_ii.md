# Maximum Subarray II
> Given an array of integers, find two non-overlapping subarrays which have the largest sum.
>
> The number in each subarray should be contiguous.
>
> Return the largest sum.
>
> Example
>
> For given [1, 3, -1, 2, -1, 2], the two subarrays are [1, 3] and [2, -1, 2] or [1, 3, -1, 2] and [2], they both have the largest sum 7.
>
> Note
>
> The subarray should contain at least one number

## Method 1: DP
### Analysis
Consider each point as potential split point for two subarray. First scan from left to right and get the maximum sum of the first [0...i] numbers. Then scan from right to left and get the maximum sum of the last [i...n] numbers. If we consider each point as a split point, then `maxSum = max(left[i] + right[i + 1]), for all possible i`.

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public int maxTwoSubArrays(ArrayList<Integer> nums) {
    int len = nums.size();
    int[] left = new int[len];
    int[] right = new int[len];
    int sum = 0;
    int maxSum = Integer.MIN_VALUE;
    // scan from left to right
    for (int i = 0; i < len; i++) {
      sum = Math.max(sum + nums.get(i), nums.get(i));
      maxSum = Math.max(maxSum, sum);
      left[i] = maxSum;
    }
    // scan from right to left
    sum = 0;
    maxSum = Integer.MIN_VALUE;
    for (int i = len - 1; i >= 0; i--) {
      sum = Math.max(sum + nums.get(i), nums.get(i));
      maxSum = Math.max(maxSum, sum);
      right[i] = maxSum;
    }
    // get the result
    maxSum = Integer.MIN_VALUE;
    for (int i = 0; i < len - 1; i++) {
      maxSum = Math.max(maxSum, left[i] + right[i + 1]);
    }
    return maxSum;
  }
}
```
