# Rotate Array

### Source
1. [leetcode](https://leetcode.com/problems/rotate-array/)

> Rotate an array of n elements to the right by k steps.
>
> For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].
>
> Note:
>
> Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.

### Analysis
Can use the algorithm shown in [here](https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/01.01.md).

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public void rotate(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0) {
      return;
    }

    k %= nums.length;
    if (k == 0) {
      return;
    }

    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
  }

  private void reverse(int[] nums, int start, int end) {
    while (start < end) {
      swap(nums, start++, end--);
    }
  }

  private void swap(int[] nums, int index1, int index2) {
    int tmp = nums[index1];
    nums[index1] = nums[index2];
    nums[index2] = tmp;
  }
}
```
