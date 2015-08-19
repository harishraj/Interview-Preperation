# Expression Tree Build
> The structure of Expression Tree is a binary tree to evaluate certain expressions. All leaves of the Expression Tree have an number string value. All non-leaves of the Expression Tree have an operator string value.
>
> Now, given an expression array, build the expression tree of this expression, return the root of this expression tree.
>
> Example
>
> For the expression (2*6-(23+7)/(1+2)) (which can be represented by ["2" "*" "6" "-" "(" "23" "+" "7" ")" "/" "(" "1" "+" "2" ")"]). The expression tree will be like
>
>                 [ - ]
>             /          \
>         [ * ]              [ / ]
>        /     \           /         \
>     [ 2 ]  [ 6 ]      [ + ]        [ + ]
>                      /    \       /      \
>                    [ 23 ][ 7 ] [ 1 ]   [ 2 ] .
>
> After building the tree, you just need to return root node [-].

## Method 1: Stack
### Analysis
Almost the same as [Expression Evaluation](expression_evaluation.md).

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
/**
 * Definition of ExpressionTreeNode:
 * public class ExpressionTreeNode {
 *     public String symbol;
 *     public ExpressionTreeNode left, right;
 *     public ExpressionTreeNode(String symbol) {
 *         this.symbol = symbol;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
  /**
   * @param expression: A string array
   * @return: The root of expression tree
   */
  public ExpressionTreeNode build(String[] expression) {
    if (expression == null || expression.length == 0) {
      return null;
    }
    Stack<ExpressionTreeNode> numStack = new Stack<>();
    Stack<String> opStack = new Stack<>();
    for (String token : expression) {
      if (isNumber(token)) {
        numStack.push(new ExpressionTreeNode(token));
      } else if (token.equals("(")) {
        opStack.push(token);
      } else if (token.equals(")")) {
        while (!opStack.peek().equals("(")) {
          numStack.push(buildNode(numStack.pop(), numStack.pop(), opStack.pop()));
        }
        opStack.pop();
      } else {
        while (!opStack.isEmpty() && getPriority(opStack.peek()) >= getPriority(token)) {
          numStack.push(buildNode(numStack.pop(), numStack.pop(), opStack.pop()));
        }
        opStack.push(token);
      }
    }
    while (!opStack.isEmpty()) {
      numStack.push(buildNode(numStack.pop(), numStack.pop(), opStack.pop()));
    }
    return numStack.isEmpty() ? null : numStack.pop();
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

  private ExpressionTreeNode buildNode(ExpressionTreeNode node2, ExpressionTreeNode node1, String op) {
    ExpressionTreeNode root = new ExpressionTreeNode(op);
    root.left = node1;
    root.right = node2;
    return root;
  }
}
```
