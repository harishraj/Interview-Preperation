# Find the Weak Connected Compoenent in the Directed Graph
## Source
[lintcode/Find the Weak Connected Component in the Directed Graph](http://www.lintcode.com/en/problem/find-the-weak-connected-component-in-the-directed-graph/)

```
Find the number Weak Connected Component in the directed graph. Each node in the graph contains a label and a list of its neighbors. (a connected set of a directed graph is a subgraph in which any two vertices are connected by direct edge path.)

Example

Given graph:

    A----->B  C
     \     |  | 
      \    |  |
       \   |  |
        \  v  v
         ->D  E <- F

Return {A,B,D}, {C,E,F}. Since there are two connected component which are {A,B,D} and {C,E,F}

Note

Sort the element in the set in increasing order
```

## Related Question
1. [Number of Islands II](http://wxx5433.github.io/number-of-islands.html)

## Method 1: Union Find
### Analysis
The graph is directed, so we cannot use the BFS as we used in the undirected graph. Union-Find algorithm can solve the problem. 

### Complexity
Time: O(NlgN)

Space: O(N)

### Code
```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
 * };
 */
public class Solution {
  /**
   * @param nodes a array of Directed graph node
   * @return a connected set of a directed graph
   */
  Map<Integer, Integer> rootMap = new HashMap<>();
  public List<List<Integer>> connectedSet2(ArrayList<DirectedGraphNode> nodes) {
    List<List<Integer>> result = new ArrayList<>();
    // initialize the node to different component.
    for (DirectedGraphNode node : nodes) {
      rootMap.put(node.label, node.label);
    }
    // union
    for (DirectedGraphNode node : nodes) {
      for (DirectedGraphNode neighbor : node.neighbors) {
        union(node.label, neighbor.label);
      }
    }
    // put nodes in the same component together.
    Map<Integer, List<Integer>> components = new HashMap<>();
    for (DirectedGraphNode node : nodes) {
      List<Integer> nodesList;
      int root = find(node.label);
      if (components.containsKey(root)) {
        nodesList = components.get(root);
      } else {
        nodesList = new ArrayList<>();
      }
      nodesList.add(node.label);
      components.put(root, nodesList);
    }
    // generate result
    for (List<Integer> nodesList : components.values()) {
      result.add(nodesList);
    }
    return result;
  }

  // Union-find methods
  private int find(int label) {
    while (label != rootMap.get(label)) {
      rootMap.put(label, rootMap.get(label));  // path compression
      label = rootMap.get(label);
    }
    return label;
  }

  private void union(int label1, int label2) {
    int root1 = find(label1);
    int root2 = find(label2);
    rootMap.put(root1, root2);
  }
}
```
