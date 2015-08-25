# Reverse Words in a String II
### Source
[leetcode](https://oj.leetcode.com/problems/reverse-words-in-a-string-ii/)

> Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.
> 
> The input string does not contain leading or trailing spaces and the words are always separated by a single space.
> 
> For example,
>
> Given s = "the sky is blue",
>
> return "blue is sky the".
> 
> Do it **in-place** without allocating extra space?

### Related Question
1. [Rotate String](rotate_string.md)
2. [Rotate Array](../array/rotate_array.md)

### Analysis
1. Reverse the whole string
2. Reverse each word 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public void reverseWords(char[] s) {
    // reverse the whole string
    reverse(s, 0, s.length - 1);
    // reverse each word
    int start = 0;
    for (int end = 0; end < s.length; end++) {
      if (s[end] == ' ') {
        reverse(s, start, end - 1);
        start = end + 1;
      }
    }
    reverse(s, start, s.length - 1);  // reverse last word
  }

  private void reverse(char[] s, int start, int end) {
    for (; start < end; start++, end--) {
      char tmp = s[start];
      s[start] = s[end];
      s[end] = tmp;
    }
  }
}
```
