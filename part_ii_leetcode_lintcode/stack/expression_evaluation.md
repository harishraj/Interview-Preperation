# Expression Evaluation
> Given an expression string array, return the final result of this expression.
>
> Example
>
> For the expression 2*6-(23+7)/(1+2), input is
>
>     [
>       "2", "*", "6", "-", "(",
>       "23", "+", "7", ")", "/",
>       (", "1", "+", "2", ")"
>     ],
>
> return 2
>
> Note
>
> The expression contains only integer, +, -, *, /, (, ).

## Method 1: Stack
### Analysis
Simulate the process to do computation. If previous operator has higher priority than the new operator, it is safe to do computation using the previous operation.

### Corner Case
1. expression = {"(", "(", ")", ")"};

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public int evaluateExpression(String[] expression) {
    if (expression == null || expression.length == 0) {
      return 0;
    }
    Stack<Integer> numStack = new Stack<>();
    Stack<String> opStack = new Stack<>();
    for (String token : expression) {
      if (isNumber(token)) {
        numStack.push(Integer.valueOf(token));
      } else if (token.equals("(")) {
        opStack.push(token);
      } else if (token.equals(")")) {
        while (!opStack.peek().equals("(")) {
          numStack.push(compute(numStack.pop(), numStack.pop(), opStack.pop()));
        }
        opStack.pop();  // pop out "("
      } else {
        while (!opStack.isEmpty() && getPriority(opStack.peek()) >= getPriority(token)) {
          numStack.push(compute(numStack.pop(), numStack.pop(), opStack.pop()));
        }
        opStack.push(token);
      }
    }
    while (!opStack.isEmpty()) {
      numStack.push(compute(numStack.pop(), numStack.pop(), opStack.pop()));
    }
    return numStack.isEmpty() ? 0 : numStack.pop();
  }

  private boolean isNumber(String token) {
    return Character.isDigit(token.charAt(0));
  }

  private int getPriority(String op) {
    if (op.equals("(")) {
      return 0;
    } else if (op.equals("+") || op.equals("-")) {
      return 1;
    } else {
      return 2;
    }
  }

  private int compute(int num2, int num1, String token) {
    int result;
    if (token.equals("+")) {
      result = num1 + num2;
    } else if (token.equals("-")) {
      result = num1 - num2;
    } else if (token.equals("*")) {
      result = num1 * num2;
    } else {
      result = num1 / num2;
    }
    return result;
  }
}
```
