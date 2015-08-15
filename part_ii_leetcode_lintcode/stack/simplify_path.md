# Simplify Path
> Given an absolute path for a file (Unix-style), simplify it.
>
> For example,
>
> path = "/home/", => "/home"
>
> path = "/a/./b/../../c/", => "/c"

## Method 1: Deque
### Analysis
We need to use a deque to store the path.

1. if dirName is "" or ".", simply ignore itt
2. if dirName is ".." and deque is not empty, remove last element in the deque
3. Otherwise, append the dirName to the tail of deque

Then we only need to iterate the deque to get the result

### Corner Case
1. When there are extra "..", the current path should be "/". For example, "/a/../.." should return "/". And "/a/../../b" should return "/b"
2. There are redundant "/". For example "/home//foo/", we should return "/home/foo". 

### Code
#### Java
```java
public class Solution {
  public String simplifyPath(String path) {
    Deque<String> deque = new LinkedList<>();
    String[] tokens = path.split("/");
    for (String token: tokens) {
      if (token.equals("..")) {
        if (!deque.isEmpty()) {
          deque.removeLast();
        }
      } else if (!token.equals(".") && !token.equals("")) {
        deque.addLast(token);
      }
    }

    StringBuilder sb = new StringBuilder();
    while (!deque.isEmpty()) {
      sb.append("/");
      sb.append(deque.removeFirst());
    }
    return sb.length() > 0 ? sb.toString() : "/";
  }
}
```

