# Min Stack
> Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
>
> push(x) -- Push element x onto stack.
>
> pop() -- Removes the element on top of the stack.
>
> top() -- Get the top element.
>
> getMin() -- Retrieve the minimum element in the stack.

## Method 1: Two Stacks
### Analysis
Since we have to implement all the operations in constant time, we must use some extra space to achieve this goal. 

The most important part here is how to implement getMin() in constant time. 

If we only use one variable to store the minimum element by now. Then after poping out this element, we can no longer tell which element is the current minimum. 

So we must use some space to store all the mimimum information. 

The idea is to use another stack to maintain the current mimimum element. When there is a new element that is smaller than or **equal to** the top of the stack(original minimum value)

  Example
  {2, 1, 1, 5}
  1. add first element. stack = {2}, minstack = {2}
  2. add 1, stack = {2, 1}, minstack = {2, 1}
  3. add 1, stack = {2, 1, 1}, minstack = {2, 1, 1}
  4. pop 1, stack = {2, 1}, minstack = {2, 1}. (Notice that if we do not push 1 into minstack in step 3, we will lose minimum value 1)
  5. add 5, stack = {2, 1, 5}, minstack = {2, 1}
  
### Complexity
Time: all operations are O(1)

Space: At the worst case O(N) when all elements have the same value.

### Code
#### Java
```java
class MinStack {
  Stack<Integer> stack = new Stack<>();
  Stack<Integer> minStack = new Stack<>();

  public void push(int x) {
    stack.push(x);
    if (minStack.isEmpty() || x <= minStack.peek()) {
      minStack.push(x);
    }
  }

  public void pop() {
    int value = stack.pop();
    if (value == minStack.peek()) {
      minStack.pop();
    }
  }

  public int top() {
    return stack.peek();
  }

  public int getMin() {
    assert !minStack.isEmpty()
    return minStack.peek();
  }
}
```
