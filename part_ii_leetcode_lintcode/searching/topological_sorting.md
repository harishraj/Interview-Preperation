# Topological Sorting
### Source 
1. [lintcode](http://www.lintcode.com/en/problem/topological-sorting/)

> Given an directed graph, a topological order of the graph nodes is defined as follow:
 
> For each directed edge A -> B in graph, A must before B in the order list.
> The first node in the order can be any node in the graph with no nodes direct to it.
> Find any topological order for the given graph.
> 
> Example
> 
> For graph as follow:
> 
> ![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcThE9AgZZszyhwe0o9qpp3VyizdIj9kWwMY50HiQEysXvkSLsoZ)
> 
> The topological order can be:
> 
> ```
> [0, 1, 2, 3, 4, 5]
> [0, 2, 3, 1, 5, 4]
> ...
> ```

## Method 1: BFS
### Analysis
It is worth noting that if there is a **cycle** in the graph, then there is no topogical sorting for the graph.

1. Get indegree of each node. 
2. Start from each indegree node, and remove it from the graph, update all its neighbors indegree by decreasing 1. 
3. Continue removing zero-indegree node from the graph until done.

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
/**
* Definition for Directed graph.
* class DirectedGraphNode {
*   int label;
*   ArrayList<DirectedGraphNode> neighbors;
*   DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
* };
*/
public class Solution {
  /**
   * @param graph: A list of Directed graph node
   * @return: Any topological order for the given graph.
   */    
  public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
    ArrayList<DirectedGraphNode> result = new ArrayList<>();
    if (graph == null) {
      return result;
    }
    // init inDegreesMap
    Map<DirectedGraphNode, Integer> inDegreesMap = new HashMap<>();
    for (DirectedGraphNode node : graph) {
      inDegreesMap.put(node, 0);
    }
    // count indegrees for all nodes
    for (DirectedGraphNode node : graph) {
      for (DirectedGraphNode neighbor : node.neighbors) {
        inDegreesMap.put(neighbor, inDegreesMap.get(neighbor) + 1);
      }
    }
    // Put all initially zero indegree node into a queue
    Queue<DirectedGraphNode> zeroInDegreeNodesQueue = new LinkedList<>();
    for (Map.Entry<DirectedGraphNode, Integer> entry : inDegreesMap.entrySet()) {
      if (entry.getValue() == 0) {
        zeroInDegreeNodesQueue.offer(entry.getKey());
      }
    }
    // Remove zero indegree nodes from the graph, and put new zero indegree node into queue.
    while (!zeroInDegreeNodesQueue.isEmpty()) {
      DirectedGraphNode zeroInDegreeNode = zeroInDegreeNodesQueue.poll();
      result.add(zeroInDegreeNode);
      for (DirectedGraphNode neighbor : zeroInDegreeNode.neighbors) {
        inDegreesMap.put(neighbor, inDegreesMap.get(neighbor) - 1);
        if (inDegreesMap.get(neighbor) == 0) {
          zeroInDegreeNodesQueue.add(neighbor);
        }
      }
    }
    assert result.size() == graph.size();
    return result;
  }
}
```

## Method 2: DFS
### Analysis
1. Get indegree of each node. 
2. Loop through all nodes, if the indegree is zero, add it to the result. And updating indegree of its neighbors by decreasing 1. 
3. If the neighbor's indegree goes to 0, recursively removing it from the graph and adding to the result.

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
/**
* Definition for Directed graph.
* class DirectedGraphNode {
*   int label;
*   ArrayList<DirectedGraphNode> neighbors;
*   DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
* };
*/
public class Solution {
  /**
   * @param graph: A list of Directed graph node
   * @return: Any topological order for the given graph.
   */    
  private Map<DirectedGraphNode, Integer> inDegreesMap = new HashMap<>();

  public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
    ArrayList<DirectedGraphNode> result = new ArrayList<>();
    if (graph == null) {
      return result;
    }
    // init inDegreesMap
    for (DirectedGraphNode node : graph) {
      inDegreesMap.put(node, 0);
    }
    // count indegrees for all nodes
    for (DirectedGraphNode node : graph) {
      for (DirectedGraphNode neighbor : node.neighbors) {
        inDegreesMap.put(neighbor, inDegreesMap.get(neighbor) + 1);
      }
    }
    // DFS
    for (DirectedGraphNode node : graph) {
      if (inDegreesMap.get(node) == 0) {
        dfs(result, node);
      }
    }
    assert result.size() == graph.size();
    return result;
  }

  private void dfs(ArrayList<DirectedGraphNode> result, DirectedGraphNode zeroInDegreeNode) {
    inDegreesMap.put(zeroInDegreeNode, -1);  // mark as visited
    result.add(zeroInDegreeNode);
    for (DirectedGraphNode neighbor : zeroInDegreeNode.neighbors) {
      inDegreesMap.put(neighbor, inDegreesMap.get(neighbor) - 1);
      if (inDegreesMap.get(neighbor) == 0) {
        dfs(result, neighbor);
      }
    }
  }
}
```
