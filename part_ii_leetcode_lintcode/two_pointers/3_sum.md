# 3 Sum
### Source
1. [lintcode](http://www.lintcode.com/en/problem/3-sum/)
2. [leetcode](https://leetcode.com/problems/3sum/)

> Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
>
> Note:
>
> Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)
>
> The solution set must not contain duplicate triplets.
>
>     For example, given array S = {-1 0 1 2 -1 -4},
>
>     A solution set is:
>     (-1, 0, 1)
>     (-1, -1, 2)

### Analysis
We should first sort the array, and then we iterate the first number, and set one pointer to the next number, and the other pointer to the last number. 

But we should be careful to deal with duplicate result. If there are duplicate number, we can skip them directly. 

### Complexity
Time: O(n^2)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public List<List<Integer>> threeSum(int[] numbers) {
    List<List<Integer>> result = new ArrayList<>();
    if (numbers == null || numbers.length < 3) {
      return result;
    }
    Arrays.sort(numbers);
    int len = numbers.length;
    for (int i = 0; i < len - 2; i++) {
      if (i > 0 && numbers[i] == numbers[i - 1]) {  /* avoid duplicates */
        continue;
      }
      int start = i + 1;
      int end = len - 1;
      while (start < end) {
        /* avoid duplicates */
        if (start > i + 1 && numbers[start] == numbers[start - 1]) {
          start++;
        } else if (end < len - 1 && numbers[end] == numbers[end + 1]) {
          end--;
        } else {
          int sum = numbers[i] + numbers[start] + numbers[end];  /* may overflow */
          if (sum < 0) {
            start++;
          } else if (sum > 0) {
            end--;
          } else {
            result.add(Arrays.asList(numbers[i], numbers[start++], numbers[end--]));
          }
        }
      }
    }
    return result;
  }
}
```


