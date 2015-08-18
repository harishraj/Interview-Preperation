# Minimum Window Substring
> Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).
>
> For example,
>
> S = "ADOBECODEBANC"
>
> T = "ABC"
>
> Minimum window is "BANC".
>
> Note:
>
> If there is no such window in S that covers all characters in T, return the emtpy string "".
>
> If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.

## Method 1: Sliding Window
### Analysis
The idea is to use two pointers, one to denote the start of the window, and the other to denote the end of the window. We start from the begining and move the end pointer until the window S[start, end] contians all characters in T. Then we try to shrink the window by moving the start pointer. If at a specific position, the new window cannot contains all characters in T, then this window is one of the potential minimum window. We do this until the end pointer run through the whole string S. 

For example,

1. Initially, start = end = 0
2. Move end pointer, A->D->O->B->E->C, now contains all characters in T. 
3. Shrink window by moving start pointer. now the window is [DOBEC], cannot contain all characters.
4. So one potential minimum window is __[ADOBEC]__.
5. Continue moving end pointer, C->O->D->E->B->A, now the window is [DOBECODEBA]. Noticed that we now have two Bs in the window. 
6. Shrink the window by moving start pointer, D->O->B->E->C. Since we have two Bs, when we move pass B, we can continue shrink the window since we have enough Bs to cover T. Another potential minimum window is __[CODEBA]__
7. Continue growing window, A->N->C, find another C to cover T. Now the window is [ODEBANC]
8. Shrink the window, C->O->D->E->B. One potential minimum window is __[BANC]__
9. Continue growing, now end run through all characters in S, stop!

When is a window minimum? Only when the start and end characters of the window is in T is the window minimum. Otherwise, we can always shrink it since we have extra characters in the two ends. Using this method, We first grow the window to fina a window that contain all characters in T. In this way, the end of the window is guaranteed to be one character in T. THen we try to shring the window by moving the start pointer. The potential window's first character must be character in T. In this way, we will search all window that contains all characters in T and the two end characters are characters in T. 

### Complexity
Time: O(N)

Space: O(1), we only need 256 entries to store count of ASCII characters. We can also use a hashMap to do this.

### Code
#### Java
```java
public class Solution {
  private static final int NUM_CHARACTERS = 256;

  public String minWindow(String source, String target) {
    if (source == null || target == null || source.length() < target.length()) {
      return "";
    }
    // preprocess
    int[] expectedCount = new int[NUM_CHARACTERS];
    for (char c : target.toCharArray()) {
      expectedCount[c]++;
    }
    // start matching
    int matchCount = 0;
    int[] actualCount = new int[NUM_CHARACTERS];
    int minWindowSize = Integer.MAX_VALUE;
    int minWindowBeginIndex = 0;
    int start = 0;
    int end = 0;
    while (end < source.length()) {
      char c = source.charAt(end++);
      if (expectedCount[c] > 0 && ++actualCount[c] <= expectedCount[c]) {
        matchCount++;
      }
      while (matchCount == target.length()) {
        c = source.charAt(start++);
        if (expectedCount[c] > 0 && --actualCount[c] < expectedCount[c]) {
          matchCount--;
          int windowSize = end - start + 1;
          if (windowSize < minWindowSize) {
            minWindowSize = windowSize;
            minWindowBeginIndex = start - 1;
          }
        }
      }
    }
    return minWindowSize == Integer.MAX_VALUE ? 
        "" : source.substring(minWindowBeginIndex, minWindowBeginIndex + minWindowSize);
  }
}
```
