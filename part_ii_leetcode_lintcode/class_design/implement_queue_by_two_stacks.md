# Implement Queue by Two Stacks
> Implement the following operations of a queue using stacks.
> 
>     push(x) -- Push element x to the back of queue.
>     pop() -- Removes the element from in front of queue.
>     peek() -- Get the front element.
>     empty() -- Return whether the queue is empty.
>
> Notes:
>
> You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
>
> You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

### Analysis
Make use of the nature that two opeartions of in/out stack opeartions can get the order require by a FIFO order. 

For example, push(1), push(2), push(3), result in {1, 2, 3} in stack, where 3 is the stack top. 

However, push it to another stack result in {3, 2, 1}, which is the desired FIFO order.

### Complexity
Amortized cost: O(1), since each element is only moved once from inStack to outStack.

### Code
```java
class MyQueue {
  private Stack<Integer> inStack = new Stack<>();
  private Stack<Integer> outStack = new Stack<>();

  // Push element x to the back of queue.
  public void push(int x) {
    inStack.push(x);
  }

  // Removes the element from in front of queue.
  public void pop() {
    if (outStack.isEmpty()) {
      moveFromInStackToOutStack();
    }
    outStack.pop();
  }

  // Get the front element.
  public int peek() {
    if (outStack.isEmpty()) {
      moveFromInStackToOutStack();
    }
    return outStack.peek();
  }

  // Return whether the queue is empty.
  public boolean empty() {
    return inStack.isEmpty() && outStack.isEmpty();
  }

  // pop all elements from inStack into outStack.
  private void moveFromInStackToOutStack() {
    while (!inStack.isEmpty()) {
      outStack.push(inStack.pop());
    }
  }
}
```

