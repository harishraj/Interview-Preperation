# 3 Sum Smaller
### Source
1. [leetcode](https://leetcode.com/problems/3sum-smaller/)

> Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.
> 
> For example, given nums = [-2, 0, 1, 3], and target = 2.
>
> Return 2. Because there are two triplets which sums are less than 2:
>
>     [-2, 0, 1]
>     [-2, 0, 3]
>
> Follow up:
>
> Could you solve it in O(n2) runtime?

### Analysis
Almost the same as [3 Sum](3_sum.md). Although we need to find a triple i, j, k with 0 <= i < j < k < n, we can actually sort the array, since the order of these three numbers doesn't matter at all. 

### Complexity
Time: O(n^2)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int threeSumSmaller(int[] nums, int target) {
    Arrays.sort(nums);
    int len = nums.length;
    int numSolutions = 0;
    for (int i = 0; i < len - 2; i++) {
      int start = i + 1;
      int end = len - 1;
      while (start < end) {
        int sum = nums[i] + nums[start] + nums[end];
        if (sum < target) {
          numSolutions += end - start;
          start++;
        } else {
          end--;
        }
      }
    }
    return numSolutions;
  }
}
```
