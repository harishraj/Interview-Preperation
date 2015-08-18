# Add Binary
> Given two binary strings, return their sum (also a binary string).
>
> For example,
>
> a = "11"
>
> b = "1"
>
> Return "100".

### Analysis
Be very careful about the index of the array. 

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java Version 1
public class Solution {
  private static final int BASE = 2;

  public String addBinary(String a, String b) {
    StringBuilder sb = new StringBuilder();
    int carry = 0;
    int resultLen = Math.max(a.length(), b.length());
    for (int i = 0; i < resultLen; i++) {
      int bit1 = i < a.length() ? a.charAt(a.length() - 1 - i) - '0' : 0;
      int bit2 = i < b.length() ? b.charAt(b.length() - 1 - i) - '0' : 0;
      int sum = carry + bit1 + bit2;
      sb.append(sum % BASE);
      carry = sum / BASE;
    }
    if (carry != 0) {
      sb.append(carry);
    }
    return sb.reverse().toString();
  }
}

#### Java Version 2
```java
public class Solution {
  private static final int BASE = 2;

  public String addBinary(String a, String b) {
    int carry = 0;
    int index1 = a.length() - 1;
    int index2 = b.length() - 1;
    StringBuilder sb = new StringBuilder();
    while (index1 >= 0 || index2 >= 0 || carry > 0) {
      if (index1 >= 0) {
        carry += a.charAt(index1--) - '0';
      }
      if (index2 >= 0) {
        carry += b.charAt(index2--) - '0';
      }
      sb.append(carry % BASE);
      carry /= BASE;
    }

    return sb.reverse().toString();
  }
}
```

