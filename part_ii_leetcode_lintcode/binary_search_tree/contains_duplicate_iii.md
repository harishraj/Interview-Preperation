# Contains Duplicate III
### Source
1. [leetcode](https://leetcode.com/problems/contains-duplicate-iii/)

> Given an array of integers, find out whether there are two distinct indices i and j in the array such that the difference between nums[i] and nums[j] is at most t and the difference between i and j is at most k.

## Method 1: TreeSet
### Analysis
The naive method for each element, check the previous k element before it. The time complexity of thi smethod is O(kn).

One way to improve on this is to use proper data structure that reduce the time to find out that there is a number satisfying the requirement in the k previous numbers.

The idea is to use binary search tree to accelerate lookup process. First we find the smallest number that is greater than or equal to nums[i] - t. Then we check that whether the number is smaller than or equal to nums[i] + t. 

The reason why we want to find the smallest number that is greater than or equal to nums[i] - t is that it provides the maximum possibility that it is no larger than nums[i] + t. If it cannot satisfy the requirement, numbers larger than it cannot satisfy as well. 

We would like to use [red-black tree](http://www.geeksforgeeks.org/red-black-tree-set-1-introduction-2/) to implement binary search tree to ensure operations take log(n) time. 

In java, TreeSet is implemented using red-black tree. 

### Corner Case
1. nums[i] + t and nums[i] - t can overflow

### Complexity
Time: O(logk * n)

Space: O(k)

### Code
#### Java
```java
public class Solution {
  public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    if (nums == null || k <= 0) {
      return false;
    }
    TreeSet<Long> appearedSet = new TreeSet<>();
    for (int i = 0; i < nums.length; i++) {
      Long potentialNum = appearedSet.ceiling((long)nums[i] - t);
      if (potentialNum != null && potentialNum <= (long)nums[i] + t) {
        return true;
      }
      if (i >= k) {
        appearedSet.remove((long)nums[i - k]);
      }
      appearedSet.add((long)nums[i]);
    }
    return false;
  }
}
```

## Method 2: Bucket 
We can make use of the idea of bucket to reduce comparison. More specifically, we create a bucket of size "t+1", there are three cases that we can find the numbers that satisfy the condition:

1. The new number fall within a bucket that already exist. (Bucket size is t + 1)
2. bucket - 1 exists, and `number - map.get(bucket - 1) <= t`. (map.get(bucket - 1) get the number that is in the bucket)
3. bucket + 1 exists, and `map.get(bucket + 1) - number <= t`.

Now, for each number, we at most need to check with 3 numbers, which reduces the time complexity to O(3N).

### Corner Case
1. Negative numbers are allowed. So we need to map them to positive number. 
2. k = 0

### Complexity
Time: O(3N)

Space: O(k)

### Code
#### Java
```java
public class Solution {
  public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    if (nums== null || k <= 0) {
      return false;
    }

    Map<Long, Long> map = new HashMap<>();
    for (int i = 0; i < nums.length; ++i) {
      long remapperNum = (long)nums[i] - Integer.MIN_VALUE;
      long bucket = remapperNum / ((long)t + 1);
      if (map.containsKey(bucket)
          || (map.containsKey(bucket - 1) && remapperNum - map.get(bucket - 1) <= t)
          || (map.containsKey(bucket + 1) && map.get(bucket + 1) - remapperNum <= t)) {
        return true;
      }
      if (i >= k) {
        long bucketToRemove = ((long)nums[i - k] - Integer.MIN_VALUE) / ((long)t + 1);
        map.remove(bucketToRemove);
      }
      map.put(bucket, remapperNum);
    }
    return false;
  }
}
```
