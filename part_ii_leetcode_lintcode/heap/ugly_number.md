# Ugly Number
> Ugly number is a number that only have factors 3, 5 and 7.
>
> Design an algorithm to find the Kth ugly number. The first 5 ugly numbers are 3, 5, 7, 9, 15 ...
>
> Example
>
> If K=4, return 9.

## Method 1: Min Heap
### Analysis
Every time get the smallest number from the min heap and multiply it by 3, 5, 7, and put the result into the heap if there haven't existed before. 

### Complexity
Time: O(klgk), each operation to the heap is O(lgk) time

Space: O(k)

### Code
#### Java
```java
public class Solution {
  /**
   * @param k: The number k.
   * @return: The kth prime number as description.
   */
  public long kthPrimeNumber(int k) {
    long[] primeFactors = {3L, 5L, 7L};
    Set<Long> visitedSet = new HashSet<>();
    PriorityQueue<Long> pq = new PriorityQueue<>();
    pq.add(1L);
    long min = 0;
    for (int i = 0; i <= k; i++) {
      min = pq.remove();
      for (long primeFactor : primeFactors) {
        long number = min * primeFactor;
        if (!visitedSet.contains(number)) {
          visitedSet.add(number);
          pq.add(number);
        }
      }
    }
    return min;
  }
}
```

## Method 2: DP
### Analysis
The new number must be the smallest one generated by multiply 3, 5, 7 to the existing number already in the array. 

We can keep 3 pointers keep track of the numbers that need to be multiple by 3, 5, 7. when the smallest number is generated by 3/5/7, updating the corresponding pointer by one, because each exising number is only meaningful to be multiplied by the same factor once.

We cannot use if else for checking which prime number (3, 5, or 7) yields the smallest product as there may be duplicates. (e.g. 3 x 5 == 5 x 3)

### Complexity
Time: O(k)

Space: O(k)

### Code
#### Java
```java
public class Solution {
  /**
   * @param k: The number k.
   * @return: The kth prime number as description.
   */
  public long kthPrimeNumber(int k) {
    long[] uglyNumbers = new long[k + 1];
    uglyNumbers[0] = 1;
    int i3 = 0;
    int i5 = 0;
    int i7 = 0;
    for (int i = 1; i <= k; i++) {
      uglyNumbers[i] = Math.min(uglyNumbers[i3] * 3, 
          Math.min(uglyNumbers[i5] * 5, uglyNumbers[i7] * 7));
      if (uglyNumbers[i] / uglyNumbers[i3] == 3) {
        i3++;
      }
      if (uglyNumbers[i] / uglyNumbers[i5] == 5) {
        i5++;
      }
      if (uglyNumbers[i] / uglyNumbers[i7] == 7) {
        i7++;
      }
    }
    return uglyNumbers[k];
  }
}
```