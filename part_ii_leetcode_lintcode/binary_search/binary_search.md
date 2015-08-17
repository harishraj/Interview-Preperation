# Binary Search
> For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n) time complexity.
>
> If the target number does not exist in the array, return -1.
>
> Example
>
> If the array is [1, 2, 3, 3, 4, 5, 10], for given target 3, return 2.

### Analysis
Use variant of binary search. If the target number is found. If the previous number doest not equal to target, then it is the result. Otherwise, continue searching in the left side.

### Complexity
Time: O(lgN)

Space: O(1)

### Code
#### Java
```java
class Solution {
  public int binarySearch(int[] nums, int target) {
    int start = 0;
    int end = nums.length - 1;
    int mid = 0;
    while (start <= end) {
      mid = start + (end - start) / 2;
      if (nums[mid] == target && (mid == 0 || nums[mid - 1] != target)) {
        break;
      } else if (nums[mid] >= target) {
        end = mid - 1;
      } else {
        start = mid + 1;
      }
    }
    return nums[mid] == target ? mid : -1;
  }
}
```
