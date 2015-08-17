# Maximum Subarray
> Find the contiguous subarray within an array (containing at least one number) which has the largest sum.
>
> For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
>
> the contiguous subarray [4,−1,2,1] has the largest sum = 6.

## Method 1: Greedy
### Analysis
We can use greedy algorithm to solve this problem. curMax represents the largest sum from A[0...n]. If the largest sum of A[0...n] is greater than zero, we may increase the sum by adding it to the new coming value. 

### Complexity
Time: O(N)

Space: O(1)

### Code
```java
public class Solution {
  public int maxSubArray(int[] nums) {
    int maxSum = Integer.MIN_VALUE;
    int curMax = 0;
    for (int num : nums) {
      curMax = Math.max(curMax + num, num);
      maxSum = Math.max(maxSum, curMax);
    }

    return maxSum;
  }
}
```

