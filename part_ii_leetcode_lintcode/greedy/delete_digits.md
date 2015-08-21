# Delete Digits
> Given string A representative a positive integer which has N digits, remove any k digits of the number, the remaining digits are arranged according to the original order to become a new positive integer.
> 
> Find the **smallest** integer after remove k digits.
>
> N <= 240 and k <= N,
>
> Example
>
> Given an integer A = "178542", k = 4
>
> return a string "12"

## Method 1: Greedy
### Analysis
The idea is to replace the larger number with the smaller number after it. 

For example, A = "178542", k = 4
1. i = 0, num = 1
2. i = 1, num = 17
3. i = 2, num = 178
4. i = 3, since the new digit is smaller than 7 and 8, we can delete it from the result, now num = 15. The reason we can do this is because we need to delete 4 digits totally, and by replacing 7 and 8 with 5, we make the current number smallest.
5. i = 4, num = 14, replace 5 with 4. We already deleted 3 digits
6. i = 5, num = 12, replace 4 with 2. Deleted 4 digits.

So the final result is 12

### Complexity
Time: O(n)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  /**
   *@param A: A positive integer which has N digits, A is a string.
   *@param k: Remove k digits.
   *@return: A string
   */
  public String DeleteDigits(String A, int k) {
    Stack<Integer> stack = new Stack<>();
    for (char c : A.toCharArray()) {
      int num = c - '0';
      while (k > 0 && !stack.isEmpty() && num < stack.peek()) {
        stack.pop();
        k--;
      }
      stack.push(num);
    }
    while (k > 0) {
      stack.pop();
      k--;
    }
    // construct result
    StringBuilder sb = new StringBuilder();
    while (!stack.isEmpty()) {
      sb.append(stack.pop());
    }
    String result = sb.reverse().toString();
    // skip leading 0
    int start = 0;
    while (result.charAt(start) == '0') {
      start++;
    }
    // handle special case
    if (start == result.length()) {
      return "0";
    }
    return result.substring(start);
  }
}
```

