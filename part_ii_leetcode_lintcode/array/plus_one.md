# Plus One
> Given a non-negative number represented as an array of digits, plus one to the number.
>
> The digits are stored such that the most significant digit is at the head of the list.
>
> Example
>
> Given [1,2,3] which represents 123, return [1,2,4].
> 
> Given [9,9,9] which represents 999, return [1,0,0,0].

### Analysis
The only case we should pay attention to is that when we have carry after adding the most significant digit. In this case, we should copy the whole array, and put the carry at the head of the array.

### Complexity
Time: O(N)

Space: O(1)

### Code
```java
public class Solution {
  public int[] plusOne(int[] digits) {
    int carry = 1;
    int[] tmp = new int[digits.length];
    for (int i = digits.length - 1; i >= 0; i--) {
      int sum = carry + digits[i];
      tmp[i] = sum % 10;
      carry = sum / 10;
    }
    int[] result = tmp;
    if (carry != 0) {
      result = new int[tmp.length + 1];
      result[0] = carry;
      for (int i = 0; i < tmp.length; i++) {
        result[i + 1] = tmp[i];
      }
    }
    return result;
  }
}
```

