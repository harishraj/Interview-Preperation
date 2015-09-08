# Subarray Sum Closest
### Source
1. [lintcode](http://www.lintcode.com/en/problem/subarray-sum-closest/)

> Given an integer array, find a subarray with sum closest to zero. Return the indexes of the first number and last number.
>
> Example
>
> Given [-3, 1, 1, -3, 5], return [0, 2], [1, 3], [1, 1], [2, 2] or [0, 4]

### Analysis
The problem is quite different from [Subarray Sum](../hash_table/subarray_sum.md), which uses hash table to solve the problem. 

Instead, we use the follow algorithm:

1. compute sum[0], sum[1], ..., sum[n], where sum[i] denotes sum up num[0] to num[i].
2. Sort the sum and preserve index information.
3. Get the min difference between sum.
4. Extract the index information and return result.

### Complexity
Time: O(nlgn)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  public ArrayList<Integer> subarraySumClosest(int[] nums) {
    ArrayList<Integer> result = new ArrayList<>();
    Pair[] sumIndexArr = new Pair[nums.length + 1];
    sumIndexArr[0] = new Pair(0, -1);
    for (int i = 0; i < nums.length; i++) {
      sumIndexArr[i + 1] = new Pair(sumIndexArr[i].sum + nums[i], i);
    }
    Arrays.sort(sumIndexArr);
    int minDiff = Integer.MAX_VALUE;
    int minIndex = 0;
    for (int i = 1; i <= nums.length; i++) {
      int diff = sumIndexArr[i].sum - sumIndexArr[i - 1].sum;
      if (diff < minDiff) {
        minDiff = diff;
        minIndex = i - 1;
      }
    }
    if (sumIndexArr[minIndex].index < sumIndexArr[minIndex + 1].index) {
      result.add(sumIndexArr[minIndex].index + 1);
      result.add(sumIndexArr[minIndex + 1].index);
    } else {
      result.add(sumIndexArr[minIndex + 1].index + 1);
      result.add(sumIndexArr[minIndex].index);
    }
    return result;
  }

  class Pair implements Comparable<Pair> {
    int sum;
    int index;

    public Pair(int sum, int index) {
      this.sum = sum;
      this.index = index;
    }

    @Override
    public int compareTo(Pair that) {
      return this.sum - that.sum;
    }
  }
}
```
