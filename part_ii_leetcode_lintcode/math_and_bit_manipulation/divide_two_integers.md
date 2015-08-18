# Divide Two Integers
> Divide two integers without using multiplication, division and mod operator.
>
> If it is overflow, return 2147483647

## Method 1: Bit Manipulation
### Analysis
The most naive method is to keep subtracting divisor from the dividend and add 1 to the result. However, if the dividend is large while divisor is small, then it may take a long time to compute result. 

For example, if dividend = Integer.MAX_VAULE, divisor = 1.  It takes 2^31 - 1 iteration to do the computation!

So instead of subtracting divisor in each iteration, we left shift the divisor(which equals to multiply by 2) until we find a number that is just <=dividend. This means that if we shift this number left one more step, it will be greater than the dividend. Then we can subtract this number from the dividend and update the result. 

### Corner Case
1. *dividend = Integer.MAX_VALUE*, *divisor = 1*. When *step = 30*, *divisor = 1 << 30*, which is still small than dividend. But if we shift the divisor left by one more step, we get *divisor = 1 << 31*, which equals to __Integer.MIN_VALUE__. So we should use **long** instead of **int** to represent the absolute value. Notice that java's long type is platform independent. It's between [2^63 - 1, - 2^63]. 
2. dividend = Integer.MIN_VALUE, divisor = -1. The result should positively overflow. 

### Complexity
Time: O(lgn)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int divide(int dividend, int divisor) {
    if (divisor == 0 || (dividend == Integer.MIN_VALUE && divisor == -1)) {
      return Integer.MAX_VALUE;
    }
    long absDividend = Math.abs((long)dividend);
    long absDivisor = Math.abs((long)divisor);
    int result = 0;
    while (absDividend >= absDivisor) {
      int step = 0;
      while ((absDivisor << (step + 1)) <= absDividend) {
        step++;
      }
      result += 1 << step;
      absDividend -= (absDivisor << step);
    }
    return (dividend < 0 ^ divisor < 0) ? -result : result;
  }
}
```

