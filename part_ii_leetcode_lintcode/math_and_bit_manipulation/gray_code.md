# Gray Code
> The gray code is a binary numeral system where two successive values differ in only one bit.
>
> Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.
>
> For example, given n = 2, return [0,1,3,2]. Its gray code sequence is:
>
>    00 - 0
>    01 - 1
>    11 - 3
>    10 - 2
> Note:
>
> For a given n, a gray code sequence is not uniquely defined.
>
> For example, [0,2,3,1] is also a valid gray code sequence according to the above definition.
>
> For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

### Problem
1. What to return if n = 0

### Analysis
We can construct gray code from i = 1 to i = n. Each time, we use (1 << i - 1) to bit-or the exsiting numbers from the last one to the first one. Supposed n = 3. 

1. Initially, {0}
2. bit = 1 << 0, {0, 1}
3. bit = 1 << 1, {00, 01, 11, 10}
4. bit = 1 << 2, {000, 001, 011, 010, 110, 111, 101, 100}

### Code
#### Java
```java
public class Solution {
  public List<Integer> grayCode(int n) {
    assert n >= 0;
    List<Integer> result = new ArrayList<>();
    result.add(0);
    for (int i = 0; i < n; ++i) {
      int size = result.size();
      int bit = 1 << i;
      for (int j = size - 1; j >= 0; --j) {
        result.add(result.get(j) | bit);
      }
    }
    return result;
  }
}
```

