# Sort Colors
### Source
1. [leetcode](https://leetcode.com/problems/sort-colors/)
2. [lintcode](http://www.lintcode.com/en/problem/sort-colors/)

> Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.
>
> Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.
>
> Note:
>
> You are not suppose to use the library's sort function for this problem.
>
> Follow up:
>
> A rather straight forward solution is a two-pass algorithm using counting sort.
>
> First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
>
> Could you come up with an one-pass algorithm using only constant space?

## Method 1: Two Pointers
### Analysis
One points at the begin of the array, the other points at the end of the array. We use another pointer to scan the array. Each time we found a 0 or 2, we swap it with the zeroPointer or the twoPointer. In this way, we swap all 0s to the beginning of the array and swap all 2s to the end of the array. Then all 1s are in the right place. 

This is actually a special case of Method 3.

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
class Solution {
  public void sortColors(int[] nums) {
    int start = 0;
    int end = nums.length - 1;
    int index = 0;
    while (index <= end) {
      if (nums[index] == 0) {
        swap(nums, start++, index++);
      } else if (nums[index] == 1) {
        index++;
      } else {
        swap(nums, end--, index);
      }
    }
  }

  private void swap(int[] nums, int index1, int index2) {
    int tmp = nums[index1];
    nums[index1] = nums[index2];
    nums[index2] = tmp;
  }
}
```

## Method 2: Three Pointers
### Analysis
Initially all points to the begining of the array. Then we scan the array from the begin to the end. Each time we meet a zero, we increament all three pointers; if it is 1, we increament onePointer and twoPointer; if it is 2, we only increament twoPointer. 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
class Solution {
  public void sortColors(int[] nums) {
    int index1 = 0;
    int index2 = 0;
    int index3 = 0;
    for (int num : nums) {
      if (num == 0) {
        nums[index3++] = 2;
        nums[index2++] = 1;
        nums[index1++] = 0;
      } else if (num == 1) {
        nums[index3++] = 2;
        nums[index2++] = 1;
      } else {
        nums[index3++] = 2;
      }
    }
  }
}
```

## Method 3: 3-way QuickSort
### Analysis
This method can also be uesd to sort array with many duplicate keys. (The number of distince keys can be arbitray, not limited to 3). In the extreme case, The idea is that each time we select the first element as v, and we put all elements smaller than v before all elements equal to v, and put all elements greater than v after all elements equal to v. In this way, the elements equal to v are in the right space. So we can recursive sort the parts smaller than v, and sort the parts greater than v. 

### Complexity
Time: O(N)

Space: O(k), in this case, k = 3

### Code
```java
public class Solution {
  public void sortColors(int[] A) {
    /* actually this problem can be solved by 3-way quick-sort */
    quickSort(A, 0, A.length - 1);
  }
  
  private void quickSort(int[] A, int lo, int hi) {
    if (lo >= hi) {
      return ;
    }
    int lt = lo, gt = hi;
    int i = lo + 1;
    int v = A[lo];
    while (i <= gt) {
      if (A[i] < v) {
        swap(A, i++, lt++);
      } else if (A[i] > v) {
        swap(A, i, gt--);
      } else {
        ++i;
      }
    }
    quickSort(A, lo, lt - 1);
    quickSort(A, gt + 1, hi);
  }
  
  private void swap(int[] A, int i, int j) {
    int tmp = A[i];
    A[i] = A[j];
    A[j] = tmp;
  }
}
```

