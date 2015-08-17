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

## 2. Extension
This is an interview question I had with BloomReach. The string can contains other characters in addition to braces. For example "if main() {}" is valid. 

In addition, there are many different types of braces. 

### Analysis
Since there are many different types of braces, it is important to make the code easy to change. We can put all possible mapping of braces in a CSV file and read it into a map. 

### Complexity
Time: O(N)

Space: O(N), there are some additional costs to store vaild types of parentheses.

### Code
#### Java
```java
public class Solution {
  private static final Map<Character, Character> leftToRightBraces = new HashMap<>();
  private static final Set<Character> leftBracesSet = new HashSet<>();
  private static final Set<Character> rightBracesSet = new HashSet<>();

  static {
    // It is possible to change this to read from, for example a CSV file, to get 
    // more kinds of braces. The benefit of this is that you don't need to change 
    // other code. Simply changes the CSV file, and anything else remains the same. 
    leftToRightBraces.put('(', ')');
    leftToRightBraces.put('[', ']');
    leftToRightBraces.put('{', '}');
    for (char c : new char[] {'(', '[', '{'}) {
      leftBracesSet.add(c);
    }
    for (char c : new char[] {')', ']', '}'}) {
      rightBracesSet.add(c);
    }
  }

  public boolean isValidParentheses(String s) {
    Stack<Character> stack = new Stack<>();
    for (char c : s.toCharArray()) {
      if (isLeftBrace(c)) {
        stack.push(c);
      } else if (isRightBrace(c)) {
        if (stack.isEmpty() || !isMatch(stack.pop(), c)) {
          return false;
        }
      }
    }
    return stack.isEmpty();
  }

  private boolean isLeftBrace(char c) {
    return leftBracesSet.contains(c);
  }

  private boolean isRightBrace(char c) {
    return rightBracesSet.contains(c);
  }

  private boolean isMatch(char c1, char c2) {
    if (leftToRightBraces.containsKey(c1)) {
      return leftToRightBraces.get(c1) == c2;
    }
    return false;
  }
}
```

