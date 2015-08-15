# Evaluate Reverse Polish Notation
## Source
1. [leetcode/Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
2. [lintcode/Evaluate Reverse Polish Notation](http://www.lintcode.com/en/problem/evaluate-reverse-polish-notation/)

> Evaluate the value of an arithmetic expression in Reverse Polish Notation.
>
> Valid operators are +, -, *, /. Each operand may be an integer or another expression.
> 
> Some examples:
>
>  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
>  
>  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6

## Method 1: Stack
### Analysis
Use a stack.

* If the current string is an integer, we push it into the stack.
* If the current string is an operator, we pop out two numbers from the top of the stack, and do the operation on these two numbers. And then push the result back into the stack.

### Code
#### Java
```java
public class Solution {
  public int evalRPN(String[] tokens) {
    Stack<Integer> stack = new Stack<>();
    for (String token: tokens) {
      if (isOperator(token)) {
        stack.push(compute(token, stack.pop(), stack.pop()));
      } else {
        stack.push(Integer.valueOf(token));
      }
    }
    return stack.pop();
  }

  private boolean isOperator(String token) {
    if (token.equals("+") || token.equals("-") 
        || token.equals("*") || token.equals("/")) {
      return true;
    }
    return false;
  }

  private int compute(String operator, int num2, int num1) {
    int result = 0;
    switch (operator.charAt(0)) {
      case '+': result = num1 + num2; break;
      case '-': result = num1 - num2; break;
      case '*': result = num1 * num2; break;
      case '/': result = num1 / num2; break;
    }
    return result;
  }
}
```

