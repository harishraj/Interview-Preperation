# 3 Sum Closest
### Source
1. [lintcode](http://www.lintcode.com/en/problem/3-sum-closest/)
2. [leetcode](https://leetcode.com/problems/3sum-closest/)

> Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
>
>     For example, given array S = {-1 2 1 -4}, and target = 1.
>
>     The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

### Analysis
Almost the same as [3 Sum](3_sum.md).

### Corner Case
1. Potential overflow.

### Complexity
Time: O(n^2)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int threeSumClosest(int[] nums, int target) {
    assert(nums != null && nums.length >= 3);
    Arrays.sort(nums);
    int len = nums.length;
    int closetSum = 0;
    int minDiff = Integer.MAX_VALUE;
    for (int i = 0; i < len - 2; i++) {
      if (i > 0 && nums[i] == nums[i - 1]) {
        continue;
      }
      int start = i + 1;
      int end = len - 1;
      while (start < end) {
        int sum = nums[i] + nums[start] + nums[end];  /* may overflow */
        int absDiff = Math.abs(sum - target);  /* may overflow */
        if (absDiff < minDiff) {
          minDiff = absDiff;
          closetSum = sum; 
        }
        if (sum < target) {
          start++;
        } else if (sum > target) {
          end--;
        } else {
          return closetSum;
        }
      }
    }
    return closetSum;
  }
}
```
