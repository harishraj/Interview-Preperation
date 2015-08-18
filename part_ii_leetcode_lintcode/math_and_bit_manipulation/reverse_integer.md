# Reverse Integer
> Reverse digits of an integer. Returns 0 when the reversed integer overflows (signed 32-bit integer).
>
> Example
>
> Given x = 123, return 321
>
> Given x = -123, return -321

## Method 1: Use long type
### Analysis
We need to deal with overflow very carefully. 

One way is to use long type to explicitely check overflow by comparing with Integer.MAX_VALUE and Integer.MIN_VALUE.

### Complexity
Time: O(logn)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  private static final int BASE = 10;

  public int reverseInteger(int n) {
    long result = 0;
    while (n != 0) {
      result = result * BASE + n % BASE;
      n /= BASE;
    }
    return (result > Integer.MAX_VALUE || result < Integer.MIN_VALUE) ?
        0 : (int)result;
  }
}
```

## Method 2: Use Integer
### Analysis
Another way to do that is to check if the result before adding exceeds Integer.MAX_VALUE / BASE, where BASE = 10.

1. If exceeds, it overflow for sure after you add another value.
2. If not exceeds, it cannot overflow. Since the input argument's type is integer, so the new digit must be 1.

### Code
#### Java
```java
public class Solution {
  private static final int BASE = 10;
  private static final int THRESHOLD = Integer.MAX_VALUE / BASE;

  public int reverseInteger(int n) {
    int result = 0;
    while (n != 0) {
      if (Math.abs(result) > THRESHOLD) {
        result = 0;
        break;
      }
      result = result * 10 + n % BASE;
      n /= BASE;
    }
    return result;
  }
}
```

