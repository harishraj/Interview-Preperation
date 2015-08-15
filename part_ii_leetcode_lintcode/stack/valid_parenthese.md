# Valid Parenthese
> Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
>
> The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

## Method 1: Stack
### Analysis
We can use a stack to push the left parentheses into stack and pop the top element from the stack if the new character is right parentheses. 

There is a small optimization. If the element in the stack is more than the number of left characters, we can directly return false, since it is impossible to be valid. 

### Corner Case
1. more right parenthese than left parentheses, stack may become empty and poping elemenet can cause exception.
2. less right parenthese, at last, stack may not be empty

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  /**
   * @param s A string
   * @return whether the string is a valid parentheses
   */
  public boolean isValidParentheses(String s) {
    Stack<Character> stack = new Stack<>();
    for (char c : s.toCharArray()) {
      if (c == '(' || c == '[' || c == '{') {
        stack.push(c);
      } else if (stack.isEmpty() || !isMatch(stack.pop(), c)) {
        return false;
      }
    }
    return stack.isEmpty();
  }

  private boolean isMatch(char c1, char c2) {
    if ((c1 == '(' && c2 == ')') || (c1 == '[' && c2 == ']') 
        || (c1 == '{' && c2 == '}')) {
      return true;
    } 
    return false;
  }
}
```

