# Longest Substring with At Most K Distinct Characters
> Given a string s, find the length of the longest substring T that contains at most k distinct characters.
>
> Example
>
> For example, Given s = "eceba", k = 3,
>
> T is "eceb" which its length is 4.

## Method 1: Sliding Window
### Corner Case
1. s = "abc", k <= 3

### Complexity
Time: O(N)

Space: O(k)

### Code
#### Java
```java
public class Solution {
  private Map<Character, Integer> counter;

  public int lengthOfLongestSubstringKDistinct(String s, int k) {
    if (s == null) {
      return 0;
    }
    int maxLen = 0;
    int start = 0;
    int end = 0;
    counter = new HashMap<>();
    int distinctCount = 0;
    while (end < s.length()) {
      while (end < s.length() && distinctCount <= k) {
        distinctCount = incCounter(s.charAt(end++), distinctCount);
      }
      if (distinctCount <= k) {
        maxLen = Math.max(maxLen, end - start);
      } else {
        maxLen = Math.max(maxLen, end - start - 1);
        while (distinctCount > k) {
          distinctCount = decCounter(s.charAt(start++), distinctCount);
        }
      }
    }
    return maxLen;
  }

  private int incCounter(char c, int distinctCount) {
    if (counter.containsKey(c)) {
      counter.put(c, counter.get(c) + 1);
    } else {
      counter.put(c, 1);
      distinctCount++;
    }
    return distinctCount;
  }

  private int decCounter(char c, int distinctCount) {
    int count = counter.get(c) - 1;
    if (count > 0) {
      counter.put(c, count);
    } else {
      counter.remove(c);
      distinctCount--;
    }
    return distinctCount;
  }
}
```
