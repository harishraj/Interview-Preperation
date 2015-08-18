# Longest Substring Without Repeating Characters
> Given a string, find the length of the longest substring without repeating characters. 
>
> Example
>
> For example, the longest substring without repeating letters for "abcabcbb" is "abc", which the length is 3.
>
> For "bbbbb" the longest substring is "b", with the length of 1.

## Method 1: Sliding Window
### Analysis
We can use a window to scan the string. We try to grow the window when there are no duplicate characters in the window. Once there are duplicte characters, we shrink the window from front to rear until there are no duplicate characters in the window. 

For these questions, we can simple use a 256 elements boolean arrays to judge if there are duplicate letters in the window. 

### Complexity
Time: O(n), n is the length of the stirng

Space: O(1), only need a 256 boolean array

### Code
#### Java Version 1
```java
public class Solution {
  private static final int NUM_CHARACTER = 256;

  public int lengthOfLongestSubstring(String s) {
    if (s == null) {
      return 0;
    }
    int start = 0;
    boolean[] appeared = new boolean[NUM_CHARACTER];
    int maxLen = 0;
    for (int end = 0; end < s.length(); end++) {
      char c = s.charAt(end);
      if (appeared[c]) {
        while (s.charAt(start++) != c) {
          appeared[s.charAt(start - 1)] = false;
        }
      }
      appeared[c] = true;
      maxLen = Math.max(maxLen, end - start + 1);
    }
    return maxLen;
  }
}
```

#### Java Version 2
```java
public class Solution {
  private static final int NUM_CHARACTER = 256;

  public int lengthOfLongestSubstring(String s) {
    if (s == null) {
      return 0;
    }
    int start = 0;
    int end = 0;
    boolean[] appeared = new boolean[NUM_CHARACTER];
    int maxLen = 0;
    while (end < s.length()) {
      while (end < s.length() && !appeared[s.charAt(end)]) {
        appeared[s.charAt(end++)] = true;
      }
      maxLen = Math.max(maxLen, end - start);
      if (end == s.length()) {
        break;
      }
      do {
        appeared[s.charAt(start)] = false;
      } while (start < end && s.charAt(start++) != s.charAt(end));
    }
    return maxLen;
  }
}
```
