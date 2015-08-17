# Digit Counts
> Count the number of k's between 0 and n. k can be 0 - 9.
>
> Example
>
> if n=12, k=1 in [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12], we have FIVE 1's (1, 10, 11, 12)

### Analysis
We need to consider the problem digit by digit. 

Using k = 1 as an example. Let's say n = 113.

1. For base = 1, we have 113 / 1 = 113. Since the last digit is 3, it is guaranteed to cover 01, 11, 21, 31, 41...111, so we can add 12*1 ones in the least significant digits.
2. For base = 10, we have 113 / 10 = 11, Since the last digit is 1, we cannot guarantee to cover 110, 111, ..., 119. Instead we can only guarantee to cover 10, 11, ...19. So we add 1*10+4 = 14 ones. (The extra 4 is 110, 111, 112, 113)
3. For base = 100, we have 113 / 100 = 1, which cannot guranteed to cover 100-199, so we cannot add 1*100 ones. Instead, we should add 14 ones.(100-113)

### Complexity
Time: O(logN)

Space: O(1)

### Corner Case
1. Base can overflow after multiple by 10.

### Code
#### Java
```java
class Solution {
  public int digitCounts(int k, int n) {
    int count = 0;
    for (long base = 1; base <= n; base *= 10) {
      long higher = n / base;
      long lower = n % base;
      if (higher % 10 > k) {
        count += (higher / 10 + 1) * base;
      } else {
        count += (higher / 10) * base;
      }
      if (higher % 10 == k) {
        count += lower + 1;
      }
    }
    return count;
  }
}
```
