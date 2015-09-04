# Contains Duplicate II
### Source 
1. [leetcode](https://leetcode.com/problems/contains-duplicate-ii/)

> Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

### Analysis
The only thing different from [Contains Duplicate](contains_duplicate.md) is that instead of putting all previous numbers into the set, we only maintain previous k numbers in the set. 

### Complexity
Time: O(N)

Space: O(k)

### Code
#### Java
```java
public class Solution {
  public boolean containsNearbyDuplicate(int[] nums, int k) {
    if (nums == null || k <= 0) {
      return false;
    }
    Set<Integer> appearedSet = new HashSet<>(k);
    for (int i = 0; i < nums.length; i++) {
      if (appearedSet.contains(nums[i])) {
        return true;
      }
      if (i >= k) {
        appearedSet.remove(nums[i - k]);
      }
      appearedSet.add(nums[i]);
    }
    return false;
  }
}
```
