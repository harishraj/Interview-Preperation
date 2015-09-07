# Two Sum II - Input array is sorted
### Source
1. [leetcode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

> Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.
>
> The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.
>
> You may assume that each input would have exactly one solution.
>
> Input: numbers={2, 7, 11, 15}, target=9
>
> Output: index1=1, index2=2

### Analysis
Use two pointers, one to the start and one to the end. 

1. sum > target, end--
2. sum < target, start++
3. sum == target, found result

### Complexity
Time: O(n)

Space: O(1)

### Code
#### java
```java
public class Solution {
  public int[] twoSum(int[] numbers, int target) {
    int start = 0;
    int end = numbers.length - 1;
    while (start < end) {
      int sum = numbers[start] + numbers[end];  // may overflow
      if (sum > target) {
        end--;
      } else if (sum < target) {
        start++;
      } else {
        return new int[]{start + 1, end + 1};
      }
    }
    return null;
  }
}
```
