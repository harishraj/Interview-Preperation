# Route Between Two Nodes in Graph
### Source
1. [lintcode](http://www.lintcode.com/en/problem/route-between-two-nodes-in-graph/)

> Given a directed graph, design an algorithm to find out whether there is a route between two nodes.
>
> Example
>
> Given graph:
>
>      A----->B----->C
>       \     |
>        \    |
>         \   |
>          \  v
>           ->D----->E
> for s = B and t = E, return true
>
> for s = D and t = C, return false

## Method 1: DFS
### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) {
 *         label = x;
 *         neighbors = new ArrayList<DirectedGraphNode>();
 *     }
 * };
 */
public class Solution {
 /**
   * @param graph: A list of Directed graph node
   * @param s: the starting Directed graph node
   * @param t: the terminal Directed graph node
   * @return: a boolean value
   */
  public boolean hasRoute(ArrayList<DirectedGraphNode> graph, 
      DirectedGraphNode s, DirectedGraphNode t) {
    return dfs(s, t, new HashSet<DirectedGraphNode>());
  }

  private boolean dfs(DirectedGraphNode s, DirectedGraphNode t, 
      Set<DirectedGraphNode> visited) {
    if (s == t) {
      return true;
    }
    visited.add(s);
    for (DirectedGraphNode neighbor : s.neighbors) {
      if (!visited.contains(neighbor)) {
        if (dfs(neighbor, t, visited)) {
          return true;
        }
      }
    }
    return false;
  }
}
```

## Method 2: BFS
### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) {
 *         label = x;
 *         neighbors = new ArrayList<DirectedGraphNode>();
 *     }
 * };
 */
public class Solution {
 /**
   * @param graph: A list of Directed graph node
   * @param s: the starting Directed graph node
   * @param t: the terminal Directed graph node
   * @return: a boolean value
   */
  public boolean hasRoute(ArrayList<DirectedGraphNode> graph, 
      DirectedGraphNode s, DirectedGraphNode t) {
    if (s == t) {
      return true;
    }
    Queue<DirectedGraphNode> queue = new LinkedList<>();
    Set<DirectedGraphNode> visited = new HashSet<DirectedGraphNode>();
    queue.offer(s);
    visited.add(s);
    while (!queue.isEmpty()) {
      DirectedGraphNode cur = queue.poll();
      for (DirectedGraphNode neighbor : cur.neighbors) {
        if (neighbor == t) {
          return true;
        }
        if (!visited.contains(neighbor)) {
          queue.offer(neighbor);
          visited.add(neighbor);
        }
      }
    }
    return false;
  }
}
```
