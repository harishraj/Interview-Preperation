# Two Strings Are Anagrams
> Write a method anagram(s,t) to decide if two strings are anagrams or not.
>
> Example
>
> Given s="abcd", t="dcab", return true.

### Analysis
Use a counter.

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  private static final int NUM_ASCII = 256;

  public boolean anagram(String s, String t) {
    if (s == null && t == null) {
      return true;
    } else if (s == null || t == null || s.length() != t.length()) {
      return false;
    }
    int[] count = new int[NUM_ASCII];
    for (char c : s.toCharArray()) {
      ++count[c];
    }
    for (char c : t.toCharArray()) {
      if (--count[c] < 0) {
        return false;
      }
    }
    return true;
  }
}
```
