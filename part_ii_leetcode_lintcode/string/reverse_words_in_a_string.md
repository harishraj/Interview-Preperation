# Reverse Words in a String
### Source
1. [lintcode](http://www.lintcode.com/en/problem/reverse-words-in-a-string/)
2. [leetcode](https://leetcode.com/problems/reverse-words-in-a-string/)

> Given an input string, reverse the string word by word.
> 
> For example,
> 
> Given s = "the sky is blue",
> 
> return "blue is sky the".

### Analysis
Iterate the string reversely. If the current character’s previous character is ‘ ‘, then append the current word. 

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java Version 1
```java
public class Solution {
  public String reverseWords(String s) {
    if (s == null) {
      return null;
    }
    StringBuilder sb = new StringBuilder();
    int end = s.length();
    for (int start = s.length() - 1; start >= 0; --start) {
      if (s.charAt(start) == ' ') {
        end = start;
      } else if (start == 0 || s.charAt(start - 1) == ' ') {
        if (sb.length() != 0) {
          sb.append(" ");
        }
        sb.append(s.substring(start, end));
      }
    }

    return sb.toString();
  }
}
```

#### Java Version 2: Use Java split method
```java
public class Solution {
  public String reverseWords(String s) {
    if (s == null) {
      return null;
    }
    String[] words = s.split(" ");
    StringBuilder sb = new StringBuilder();
    for (int i = words.length - 1; i >= 0; i--) {
      if (!words[i].equals("")) {
        if (sb.length() != 0) {
          sb.append(" ");
        }
        sb.append(words[i]);
      }
    }
    return sb.toString();
  }
}
```
