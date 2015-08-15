# Print Numbers by Recursion
> Print numbers from 1 to the largest number with N digits by recursion.
>
> Example
>
> Given N = 1, return [1,2,3,4,5,6,7,8,9].
>
> Given N = 2, return [1,2,3,4,5,6,7,8,9,10,11,12,...,99].
>
> Note
>
> It's pretty easy to do recursion like:
>
> ``` 
> recursion(i) {
>   if i > largest number:
>     return
>   results.add(i)
>   recursion(i + 1)
> }
> ```
>
> however this cost a lot of recursion memory as the recursion depth maybe very large. Can you do it in another way to recursive with at most N depth?

### Analysis
Since we need to do it in at most N depth recursion. We can generate the number digit by digit.

### Compleixty
Time: O(10^N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  List<Integer> result;
  public List<Integer> numbersByRecursion(int n) {
    assert n >= 0;
    result = new ArrayList<>();
    numbersByRecursionHelper(n, 0);
    return result;
  }

  private void numbersByRecursionHelper(int n, int num) {
    if (n == 0) {
      if (num != 0) {
        result.add(num);
      }
      return;
    }
    for (int i = 0; i <= 9; i++) {
      numbersByRecursionHelper(n - 1, num * 10 + i);
    }
  }
}
```
