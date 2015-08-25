# Rotate String
### Source
1. [lintcode](http://www.lintcode.com/en/problem/rotate-string/)

> Given a string and an offset, rotate string by offset. (rotate from left to right)
> 
> Example
>
> Given "abcdefg".
>
> ```
> offset=0 => "abcdefg"
> offset=1 => "gabcdef"
> offset=2 => "fgabcde"
> offset=3 => "efgabcd"
> ```

### Related Question
1. [Rotate Array](../array/rotate_array.md)
2. [Reverse Words in a String II](reverse_words_in_a_string_ii.md)

### Analysis
1. Reverse the whole string
2. Reverse both parts

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public char[] rotateString(char[] A, int offset) {
    if (A == null || A.length == 0) {
      return A;
    }
    int len = A.length;
    offset %= len;
    if (offset == 0) {
      return A;
    }
    reverse(A, 0, len - offset - 1);
    reverse(A, len - offset, len - 1);
    reverse(A, 0, len - 1);
    return A;
  }
  
  private void reverse(char[] A, int start ,int end) {
    while (start < end) {
      swap(A, start++, end--);
    }
  }
  
  private void swap(char[] A, int i, int j) {
    char tmp = A[i];
    A[i] = A[j];
    A[j] = tmp;
  }
}
```
