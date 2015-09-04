# Find Peak Element
### Source
1. [leetcode](https://leetcode.com/problems/find-peak-element/)

> A peak element is an element that is greater than its neighbors.
>
> Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.
>
> The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.
>
> You may imagine that num[-1] = num[n] = -∞.
>
> For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.

## Method 1: Sequential Scan
### Analysis
The problem is like find any one of the local maximum in the array. The key point here is that since num[i] ≠ num[i+1], we are guaranteed to find one local maximum. The reason is that if the array is in ascending order, then the last element is the peak element. If the array is in descending order, then the first element is the peak element. Otherwise, there is some element in the array that breaks the order, so there must exist a peak element. For example, 1 3 4 5 2 6 7, then 5 and 7 is peak element. 

The most straightforward way to solve this problem is using sequential search. Every time, we compare the element with the previous element. If it is smaller than the previous element, then the previous element must be one of the valid peak element. If we search the whole array and cannot find one that is smaller than its previous element, then the whole array is sorted in ascending order, the last element is the peak element. 

For instance, 1 2 3 2, 2 is a peak element; 3 2 4 5, 3 is a peak element; 1 2 3 4 5, 5 is a peak element. 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int findPeakElement(int[] num) {
    for (int i = 1; i < num.length; ++i) {
      if (num[i] < num[i - 1]) {
        return i - 1;
      }
    }
    return num.length - 1;
  }
}
```

## Method 2: Binary Search
We can improve this method by using binary search, if the mid element is larger than its next element, there must exists a peak element in the first half (including the mid element). The reason is that if the fist half is in ascending order, the mid element is the peak element. Otherwise, there is some element breaks the order, and so we can find a peak element. If the mid element is smaller than the next element, we can find that a peak element in the second half (excluding the mid element). 

For instance,  1 2 3 2 4, 3 is greater than its next element 2, so there must be a peak element in [1 2 3], which is 3(greater than previous 2 and next 2); 1 4 3 2 4, the peak element in [1 4 3] is 4; 1 2 3 4 5, the peak element in [4 5] is 5; 1 2 3 5 4, the peak element in [5 4] is 5.

### Complexity
Time: O(logN)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int findPeakElement(int[] nums) {
    int start = 0;
    int end = nums.length - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;
      // since start < end, mid is at most nums.length - 2
      if (nums[mid] > nums[mid + 1]) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }
    return start;
  }
}
```
