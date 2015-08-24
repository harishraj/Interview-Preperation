# Happy Number
### Source
1. [leetcode/Happy Number](https://leetcode.com/problems/happy-number/)

> Write an algorithm to determine if a number is "happy".
>
> A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.
>
> Example: 19 is a happy number
>
>     1^2 + 9^2 = 82
>     8^2 + 2^2 = 68
>     6^2 + 8^2 = 100
>     1^2 + 0^2 + 0^2 = 1

## Method 1: Hash Set
### Analysis
Use a HashSet to store all numbers that have been visited. Once detect a duplicate, check if it is 1.

### Complexity
Space: O(1)

### Code
#### Java
```java
public class Solution {
  public boolean isHappy(int n) {
    Set<Integer> numbers = new HashSet<Integer>();
    while (numbers.add(n)) {
      int sum = 0;
      while (n != 0) {
        int tmp = n % 10;
        sum += tmp * tmp;
        n /= 10;
      }
      n = sum;
    }

    return n == 1;
  }
}
```

## Method 2: Fast Slow Pointer
### Analysis
Use the method in [Linked List Cycle](http://wxx5433.github.io/leetcode-linked-list-cycle.html). The problem actually is the same to detect the entry of a cycle. 

### Code
#### Java
```java
public class Solution {
  public boolean isHappy(int n) {
    int slow = n;
    int fast = n;
    do {
      slow = calculate(slow);
      fast = calculate(calculate(fast));
    } while (slow != fast);

    return slow == 1;
  }

  private int calculate(int num) {
    int result = 0;
    while (num != 0) {
      int tmp = num % 10;
      result += tmp * tmp;
      num /= 10;
    }

    return result;
  }
}
```

