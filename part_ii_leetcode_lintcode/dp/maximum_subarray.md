# Maximum Subarray
> Find the contiguous subarray within an array (containing at least one number) which has the largest sum.
>
> For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
>
> the contiguous subarray [4,−1,2,1] has the largest sum = 6.

### Analysis
We can use DP to solve this problem. dp[n] represents the largest sum from A[0...n]. We can update dp[n + 1] by `dp[n + 1] = Math.max(dp[n] + A[n + 1], A[n + 1])`. The meaning is that if the largest sum of A[0...n] is greater than zero, we may increase the sum by adding it to the current value. 

Instead of using an array, we can simply use a variable since we only use dp[n - 1] each time. 

### Complexity
Time: O(N)

Space: O(1)

### Code
```java
public class Solution {
  public int maxSubArray(int[] nums) {
    int maxSum = Integer.MIN_VALUE;
    int sum = 0;
    for (int num : nums) {
      sum = Math.max(sum + num, num);
      maxSum = Math.max(maxSum, sum);
    }

    return maxSum;
  }
}
```

