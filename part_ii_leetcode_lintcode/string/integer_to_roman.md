# Integer to Roman
### Source
1. [lintcode](http://www.lintcode.com/en/problem/integer-to-roman/)
2. [leetcode](https://leetcode.com/problems/integer-to-roman/)

> Given an integer, convert it to a roman numeral.
>
> Input is guaranteed to be within the range from 1 to 3999.
> 
> Examples at: http://literacy.kent.edu/Minigrants/Cinci/romanchart.htm

## Method 1
### Analysis
We can start from subtract base=1000 from number, if it is greater than base, we append the corresponding character to the result. 

However, we need to deal with two cases. 

1. num / base = 4
2. num / (base / 10) = 9

So we can deal with the problem using the following logic: (suppose roman(base) means convert the number to roman number)

1. If num is 4 times of base (base = 1000, 100, 10, 1), append "roman(base)roman(base*5)"
2. If num is greater than base, add base
3. If num is smaller than base (base = 1000, 100, 10, 1) but it is 9 times of base/10, append "roman(base/10)roman(base)"
4. Otherwise, base = next base. 

For example, number = 45

1. base = 1000
2. base = 500
3. base = 100
4. base = 50
5. base = 10, number/base == 4, so result = "XL", now number = 5
6. base = 10, number < base
7. base = 5, number >= base, so result = "XLV", now number = 0
8. stop

### Complexity
Time: In the worst case, each base will be used three times in the loop. However, it is impossible to reach the worst case, since 4500 will never be represented by "MMMDDDD", actually it exceeds the maximum value 3999. So time complexity is smaller than O(7*3)

Space: O(1), only two small arrays

### Code
#### Java
```java
public class Solution {
  private static final char[] ROMANS = {'M', 'D', 'C', 'L', 'X', 'V', 'I'};
  private static final int[] BASES = {1000, 500, 100, 50, 10, 5, 1};

  public String intToRoman(int n) {
    assert n >= 1 && n <= 3999;

    StringBuilder sb = new StringBuilder();
    int baseIndex = 0;
    while (n != 0) {
      if (baseIndex % 2 == 0 && n / BASES[baseIndex] == 4) {
        sb.append(ROMANS[baseIndex]);
        sb.append(ROMANS[baseIndex - 1]);
        n -= 4 * BASES[baseIndex];
      } else if (n >= BASES[baseIndex]) {
        sb.append(ROMANS[baseIndex]);
        n -= BASES[baseIndex];
      } else if (baseIndex % 2 == 0 && n / BASES[baseIndex + 2] == 9) {
        sb.append(ROMANS[baseIndex + 2]);
        sb.append(ROMANS[baseIndex]);
        n -= 9 * BASES[baseIndex + 2];
      } else {
        baseIndex++;
      }
    }
    return sb.toString();
  }
}
```

## Method 2
### Analysis
Get represensation of all possible combination on each digit.

### Complexity
Time: O(1)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  private static final String M[] = {"", "M", "MM", "MMM"};
  private static final String C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
  private static final String X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
  private static final String I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};

  public String intToRoman(int n) {
    assert n >= 1 && n <= 3999;
    return M[n / 1000] + C[(n % 1000) / 100] + X[(n % 100) / 10] + I[n % 10];
  }
}
```

