# Convert Expression to Polish Notation
> Given an expression string array, return the Polish notation of this expression. (remove the parentheses)
>
> Example
>
> For the expression [(5 − 6) * 7] (which represented by ["(", "5", "−", "6", ")", "*", "7"]), the corresponding polish notation is [* - 5 6 7] (which the return value should be ["*", "−", "5", "6", "7"]).
>
> Clarification
>
> Definition of Polish Notation:
>
> * http://en.wikipedia.org/wiki/Polish_notation
> * http://baike.baidu.com/view/7857952.htm

### Analysis
Need some modification on [Convert Expression to Reverse Polish Notation](convert_expression_to_reverse_polish_notation.md).

1. Need to scan the expression from right to left.
2. Each time ")" is met, put into opStack. (Instead of "(")
3. Each time "(" is met, pop from opStack until see ")". (The reason is that it evaluation from end to start)
4. Only pop op from element when priority(prevOp) > priority(op), instead of >=. The reason is the same, since we evaluate from end to start.

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  /**
   * @param expression: A string array
   * @return: The Polish notation of this expression
   */
  public ArrayList<String> convertToPN(String[] expression) {
    ArrayList<String> result = new ArrayList<>();
    if (expression == null || expression.length == 0) {
      return result;
    }
    Stack<String> opStack = new Stack<>();
    for (int i = expression.length - 1; i >= 0; i--) {
      String token = expression[i];
      if (isNumber(token)) {
        result.add(token);
      } else if (token.equals("(")) {
        while (!opStack.peek().equals(")")) {
          result.add(opStack.pop());
        }
        opStack.pop();
      } else if (token.equals(")")) {
        opStack.push(token);
      } else {
        while (!opStack.isEmpty() && getPriority(opStack.peek()) > getPriority(token)) {
          result.add(opStack.pop());
        }
        opStack.push(token);
      }
    }
    while (!opStack.isEmpty()) {
      result.add(opStack.pop());
    }
    Collections.reverse(result);
    return result;
  }

  private boolean isNumber(String token) {
    return Character.isDigit(token.charAt(0));
  }

  private int getPriority(String op) {
    if (op.equals(")")) {
      return 0;
    } else if (op.equals("+") || op.equals("-")) {
      return 1;
    } else {
      return 2;
    }
  }
}
```
