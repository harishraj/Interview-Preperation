# Factor Combinations
### Source
1. [leetcode](https://leetcode.com/problems/factor-combinations/)

> Numbers can be regarded as product of its factors. For example,
> 
> ```
> 8 = 2 x 2 x 2;
>   = 2 x 4.
> ```
>
> Write a function that takes an integer n and return all possible combinations of its factors.
> 
> Note: 
>
> 1. Each combination's factors must be sorted ascending, for example: The factors of 2 and 6 is [2, 6], not [6, 2].
> 2. You may assume that n is always positive.
> 3. Factors should be greater than 1 and less than n.
>
> Examples: 
> ```
> input: 1
> output: 
> []
> ```
>
> ```
> input: 37
> output: 
> []
> ```
>
> ```
> input: 12
> output:
> [
>   [2, 6],
>   [2, 2, 3],
>   [3, 4]
> ]
> ```
>
> ```
> input: 32
> output:
> [
>   [2, 16],
>   [2, 2, 8],
>   [2, 2, 2, 4],
>   [2, 2, 2, 2, 2],
>   [2, 4, 4],
>   [4, 8]
> ]
> ```

## Method 1: DFS
### Analysis
Using dfs to search the result. Pay special attention that in order to keep factor in ascending order, we want to pass a start parameter to the recursive function to make sure it will only try larger factors.

### Complexity
Time: O(n * logn)

Space: O(logn)

### Code
#### Java
```java
public class Solution {
  private List<List<Integer>> result;
  public List<List<Integer>> getFactors(int n) {
    result = new ArrayList<>();
    if (n <= 1) {
      return result;
    }
    dfs(new ArrayList<>(), n, 2);
    return result;
  }

  private void dfs(List<Integer> path, int n, int start) {
    if (n == 1) {
      if (path.size() > 1) {
        result.add(new ArrayList<>(path));
      }
      return;
    }
    int maxFactor = Math.max(2, (int)Math.sqrt(n));
    for (int factor = start; factor <= maxFactor; factor++) {
      if (n % factor == 0) {
        path.add(factor);
        dfs(path, n / factor, factor);
        path.remove(path.size() - 1);
      }
    }
  }
}
```
