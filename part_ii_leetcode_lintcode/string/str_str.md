# strStr
> strstr (a.k.a find sub string), is a useful function in string operation. Your task is to implement this function.
>
> For a given source string and a target string, you should output the first index(from 0) of target string in source string.
>
> If target does not exist in source, just return -1.
>
> Example
>
> If source = "source" and target = "target", return -1.
>
> If source = "abcdabcdefg" and target = "bcd", return 1.

### Analysis
Use naive method to match.

KMP can solve this problem in O(N) time.

### Complexity
Time: O(N^2)

Sapce: O(1)

### Code
#### Java
```java
class Solution {
  /**
   * Returns a index to the first occurrence of target in source,
   * or -1  if target is not part of source.
   * @param source string to be scanned.
   * @param target string containing the sequence of characters to match.
   */
  public int strStr(String source, String target) {
    if (source == null || target == null) {
      return -1;
    }
    for (int start = 0; start + target.length() <= source.length(); start++) {
      int len = 0;
      while (len < target.length() && source.charAt(start + len) == target.charAt(len)) {
        len++;
      }
      if (len == target.length()) {
        return start;
      }
    }
    return -1;
  }
}
```
