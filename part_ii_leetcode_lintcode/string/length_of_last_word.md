# Length of Last Word
> Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.
>
> If the last word does not exist, return 0.
>
> Example
>
> Given s = "Hello World", return 5.
>
> Note
>
> A word is defined as a character sequence consists of non-space characters only.

## Method 1: Split
### Analysis
If split method is allow, then simply use it and find the length of the last non-empty string.

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public int lengthOfLastWord(String s) {
    if (s == null) {
      return 0;
    }
    String[] words = s.split(" ");
    int len = 0;
    for (int i = words.length - 1; i >= 0; i--) {
      len = words[i].length();
      if (len > 0) {
        break;
      }
    }
    return len;
  }
}
```

## Method 2
### Analysis
Simply scan the string from right to left. Keep increasing the counter when a non-space character is met. If a space is encoutered and the counter is not zero, simply return the result.

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int lengthOfLastWord(String s) {
    if (s == null) {
      return 0;
    }
    int len = 0;
    for (int i = s.length() - 1; i >= 0; i--) {
      if (s.charAt(i) == ' ') {
        if (len > 0) {
          break;
        }
      } else {
        len++;
      }
    }
    return len;
  }
}
```
