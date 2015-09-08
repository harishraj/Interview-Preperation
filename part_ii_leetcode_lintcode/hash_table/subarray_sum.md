# Subarray Sum
### Source
1. [lintcode](http://www.lintcode.com/submission/1140065/)

> Given an integer array, find a subarray where the sum of numbers is zero. Your code should return the index of the first number and the index of the last number.
>
> Have you met this question in a real interview? Yes
>
> Example
>
> Given [-3, 1, 2, -3, 4], return [0, 2] or [1, 3].

### Analysis
The most naive method is to start from each element and add till the end to check if there is a continous subarray starting from this element that can sum to the given value. The time complexity is O(n^2).

However, it is possible to improve this by using a map to store the sum from index 0 to i as the key. We can denote it as sum[i]. In this way, when given a new number, we only need to check if sum[j] is already in the map, which means nums[i + 1], ..., nums[j] sum up to zero.

### Complexity
Time: O(n)

Space: O(n)

### Code
```java
public class Solution {
  public ArrayList<Integer> subarraySum(int[] nums) {
    ArrayList<Integer> result = new ArrayList<>();
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, -1);
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
      sum += nums[i];
      if (map.containsKey(sum)) {
        result.add(map.get(sum) + 1);
        result.add(i);
        break;
      }
      map.put(sum, i);
    }
    return result;
  }
}
```


