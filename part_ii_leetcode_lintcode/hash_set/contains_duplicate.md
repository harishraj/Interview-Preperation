# Contains Duplicate 
### Source
1. [leetcode](https://leetcode.com/problems/contains-duplicate/)

> Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

### Analysis
Simply add numbers seen before into a hash set, and whenever meet a new number, juged whether it is already in the set or not.

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public boolean containsDuplicate(int[] nums) {
    Set<Integer> appearedSet = new HashSet<>();
    for (int num : nums) {
      if (!appearedSet.add(num)) {
        return true;
      }
    }
    return false;
  }
}
```
