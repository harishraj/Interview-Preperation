# Sort Colors II
### Source
1. [lintcode](http://www.lintcode.com/en/problem/sort-colors-ii/)

> Given an array of n objects with k different colors (numbered from 1 to k), sort them so that objects of the same color are adjacent, with the colors in the order 1, 2, ... k.
> Example
>
> Given colors=[3, 2, 2, 1, 4], k=4, your code should sort colors in-place to [1, 2, 2, 3, 4].

## Method 1: Count sort
### Analysis
Simply use count sort.

### Complexity
Time: O(N)

Space: O(k)

### Code
#### Java
```java
class Solution {
  public void sortColors2(int[] colors, int k) {
    int[] count = new int[k];
    for (int color : colors) {
      count[color - 1]++;
    }
    int index = 0;
    for (int i = 0; i < k; i++) {
      for (int j = 0; j < count[i]; j++) {
        colors[index++] = i + 1;
      }
    }
  }
}
```

## Method 2: Bucket sort
### Analysis
The idea is to use bucket sort. Since the numbers are between 1 to k, it is possible to store the count information into the array without additional space.

The idea is to use negative number to represent the counter. For example, -1 means counter = 1.

Then for each number, we only need to update the counter in nums[nums[i] - 1].

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
The code only works when colors.length >= k.

```java
class Solution {
  public void sortColors2(int[] colors, int k) {
    for (int i = 0 ; i < colors.length; i++) {
      if (colors[i] <= 0) {
        continue;
      }
      int num = colors[i];
      if (colors[num - 1] <= 0) {  // bucket already exist
        colors[num - 1]--;
        colors[i] = 0;
      } else {  
        colors[i] = colors[num - 1];
        colors[num - 1] = -1;
        i--;
      }
    }

    int index = colors.length - 1;
    for (int i = k; i > 0; i--) {
      int count = -colors[i - 1];
      for (int j = 0; j < count; j++) {
        colors[index--] = i;
      }
    }
  }
}
```

