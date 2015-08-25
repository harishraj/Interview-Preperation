# Roman to Integer
### Source
1. [leetcode](https://leetcode.com/problems/roman-to-integer/)
2. [lintcode](http://www.lintcode.com/en/problem/roman-to-integer/)

> Given a roman numeral, convert it to an integer.
>
> Input is guaranteed to be within the range from 1 to 3999.
>
> Examples at: http://literacy.kent.edu/Minigrants/Cinci/romanchart.htm

### Analysis
We only need to scan the string once. Each time we convert the current character into its integer representation and add it to the result. If the integer representation of the current character is greater than that of the previous character, it means that we combine these two characters to represent one number, and we need to subtract the integer representation of the previous character. Since we have already add it before, we need to subtract twice of the value. 

For example, XIV

1. X -> 10, so result = 10
2. I -> 1, and prev > cur, so result += 1 = 11
3. V -> 5, and cur > prev, so result -= 1 * 2 + 5 = 14.

### Complexity
Time: O(n)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  private static final Map<Character, Integer> ROMAN_TO_INTEGER_MAP = new HashMap<>();
  static {
    ROMAN_TO_INTEGER_MAP.put('I', 1);
    ROMAN_TO_INTEGER_MAP.put('V', 5);
    ROMAN_TO_INTEGER_MAP.put('X', 10);
    ROMAN_TO_INTEGER_MAP.put('L', 50);
    ROMAN_TO_INTEGER_MAP.put('C', 100);
    ROMAN_TO_INTEGER_MAP.put('D', 500);
    ROMAN_TO_INTEGER_MAP.put('M', 1000);
  }

  public int romanToInt(String s) {
    int decimal = 0;
    for (int i = 0; i < s.length(); i++) {
      if (i > 0 && ROMAN_TO_INTEGER_MAP.get(s.charAt(i)) > ROMAN_TO_INTEGER_MAP.get(s.charAt(i - 1))) {
        decimal += ROMAN_TO_INTEGER_MAP.get(s.charAt(i)) - 2 * ROMAN_TO_INTEGER_MAP.get(s.charAt(i - 1));
      } else {
        decimal += ROMAN_TO_INTEGER_MAP.get(s.charAt(i));
      }
    }
    return decimal;
  }
}
```

