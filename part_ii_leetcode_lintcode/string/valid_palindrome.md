# Valid Palindrome 
> Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
>
> For example,
>
> "A man, a plan, a canal: Panama" is a palindrome.
>
> "race a car" is not a palindrome.
>
> Note:
>
> Have you consider that the string might be empty? This is a good question to ask during an interview.
>
> For the purpose of this problem, we define empty string as valid palindrome.

## Method 1: Two Pointer
### Analysis
Use two pointers, one at begin and one at end. Search until the two pointers cross. 

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public boolean isPalindrome(String s) {
    if (s == null) {
      return true;
    }
    int start = 0;
    int end = s.length() - 1;
    while (start < end) {
      while (start < end && !Character.isLetterOrDigit(s.charAt(start))) {
        start++;
      }
      while (start < end && !Character.isLetterOrDigit(s.charAt(end))) {
        end--;
      }
      if (Character.toLowerCase(s.charAt(start++)) != Character.toLowerCase(s.charAt(end--))) {
        return false;
      }
    }
    return true;
  }
}
```
