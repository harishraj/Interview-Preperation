# Two Sum
### Source
1. [lintcode](http://www.lintcode.com/en/problem/2-sum/)
2. [leetcode](https://leetcode.com/problems/two-sum/)

> Given an array of integers, find two numbers such that they add up to a specific target number.
>
> The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.
>
> You may assume that each input would have exactly one solution.
>
> Input: numbers={2, 7, 11, 15}, target=9
>
> Output: index1=1, index2=2

### Analysis
We cannot sort the arrays and then find the result, since it will break the ordering of the index. 

We can store the number and its index into a map each time we visit a number. When we find that `target - num` is already in the map, it means the number has appeared before. We found one Result. 

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public int[] twoSum(int[] numbers, int target) {
    Map<Integer, Integer> indexMap = new HashMap<>();
    for (int i = 0; i < numbers.length; i++) {
      if (indexMap.containsKey(target - numbers[i])) {
        return new int[]{indexMap.get(target - numbers[i]), i + 1};
      }
      indexMap.put(numbers[i], i + 1);
    }
    return null;
  }
}
```

