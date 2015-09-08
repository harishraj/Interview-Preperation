# Subarray Sum to Target Value
### Source
1. Google Interview Question

> Given an array of integers, find a continuous subarray which sums to a given number, return the <start, end> index pair. 
> 
> Example: {3, 1, 5, 6, 2, 5, 3}, target sum is 12, should return {1, 3}, which means 1+5+6=12

### Analysis
If the array only contains non-negative numbers, it can be solved using sliding window. 

We can use the same method as [Subarray Sum](subarray_sum.md). The idea is to store the sum from index 0 to i as the key. We can denote it as sum[i]. In this way, when given a new number, we only need to check if `sum[j] - target` exists in the map. If it exist in the map, it means that num[i+1], nums[i+2], ..., num[j] sums up to target.

### Complexity
Time: O(n)

Space: O(n)

### Code
```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Solution {
  public int[] findTarget(int[] nums, int target) {
    int[] result = new int[2];
    Arrays.fill(result, -1);
    if (nums == null || nums.length == 0) {
      return new int[0];
    }
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, -1);
    int totalSum = 0;
    for (int i = 0; i < nums.length; ++i) {
      totalSum += nums[i];
      if (map.containsKey(totalSum - target)) {
        result[0] = map.get(totalSum - target) + 1;
        result[1] = i;
        break;
      }
      map.put(totalSum, i);
    }
    return result;
  }

  public static void main(String[] args) {
    int[] nums = {3, 5, -1, -2, 4, 3, -2, 1};
    int[] result = new Solution().findTarget(nums, 6);
    System.out.println(result[0] + " " + result[1]);
  }
}
```

