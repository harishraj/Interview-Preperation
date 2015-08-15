# Preorder Traversal
## Analysis
It's a very natural idea to keep visiting the current node and then visit its left child. When there is no more left child, we then visit the node's right child. 

In order to do this, every time we visit a node, we push it into a stack. When we have no left child to visit, cur is set to null at this time. 

Since we have to visit the right child now, we need to pop a node from the stack, which is the least recently visited node. If the node has no right child, then we keep poping elements from the stack. The reason why we can do this is that all the elements in the stack have been visited, and their left childs have also been visited. 

## Code
### Version 1: Best for preorder
```java
public class Solution {
  public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();

    TreeNode cur = root;
    while (cur != null || !stack.isEmpty()) {
      if (cur == null) {
        cur = stack.pop();
      }
      result.add(cur.val);
      if (cur.right != null) {
        stack.add(cur.right);
      }
      cur = cur.left;
    }
    return result;
  }
}
```

### Version 2: Only need a little change to inorder
```java
public class Solution {
  public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();

    TreeNode cur = root;
    while (cur != null || !stack.isEmpty()) {
      while (cur != null) {
        result.add(cur.val);
        stack.add(cur);
        cur = cur.left;
      }
      cur = stack.pop().right;
    }

    return result;
  }
}
```

### Version 3
```java
public class Solution {
  public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();

    if (root == null) {
      return result;
    }
    stack.push(root);
    while (!stack.isEmpty()) {
      TreeNode cur = stack.pop();
      result.add(cur.val);
      if (cur.right != null) {
        stack.push(cur.right);
      }
      if (cur.left != null) {
        stack.push(cur.left);
      }
    }

    return result;
  }
}
```

# Inorder Traversal
## Analysis
We can use the same method as preorder traversal. The only thing we need to change is when we visit the node.

We keep going down from the tree to the leftmost child. And push the node along the path into the stack. When there is no more left child, it's time to pop an element from the stack and visit it. Then we should visit its right child. It does not matter if the node has right child, since if not, cur is null and we won't enter the while (cur != null) loop. So we will keep pop elements from the stack until we find a node that has right child. 

## Code
```java
public class Solution {
  public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();

    TreeNode cur = root;
    while (cur != null || !stack.isEmpty()) {
      while (cur != null) {
        stack.push(cur);
        cur = cur.left;
      }
      cur = stack.pop();
      result.add(cur.val);
      cur = cur.right;
    }

    return result;
  }
}
```

# Postorder Traversal
## Analysis
Initlially, we push the root into the stack. Then every time we peek the top element in the stack. 

There are three cases that we should visit the top element in the stack:

* no left child & no right child
* no right child & has visited left child
* has right child & has visited right child

We have already visited all the node's left and right child, so we cannot push its left and right child into the stack again. Otherwise, we will enter an infinite loop. So we continue to get next top element in the stack.

When the node cannot statisfy any one of the above three cases, we first push its *RIGHT* child into the stack, then push its left child into the stack. 

## Code
### Version 1
```java
public class Solution {
  public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) {
      return result;
    }
    Stack<TreeNode> stack = new Stack<>();
    TreeNode prev = null;
    while (root != null || !stack.isEmpty()) {
      while (root != null) {
        stack.push(root);
        if (root.right != null) {
          stack.push(root.right);
        }
        root = root.left;
      }
      root = stack.pop();
      if ((root.left == null && root.right == null)
          || (prev != null && (prev == root.left || prev == root.right))) {
        result.add(root.val);
        prev = root;
        root = null;
      }
    }
    return result;
  }
}
```

### Version 2
```java
public class Solution {
  public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();

    if (root == null) {
      return result;
    }

    stack.push(root);
    TreeNode prev = null;
    while (!stack.isEmpty()) {
      TreeNode cur = stack.peek();
      if ((cur.left == null && cur.right == null)
          || (cur.right == null && prev == cur.left) 
          || (cur.right != null && prev == cur.right)) {
        result.add(cur.val);
        prev = cur;
        stack.pop();
      } else {
        if (cur.right != null) {
          stack.push(cur.right);
        }
        if (cur.left != null) {
          stack.push(cur.left);
        }
      } 
    }

    return result;
  }
}
```
