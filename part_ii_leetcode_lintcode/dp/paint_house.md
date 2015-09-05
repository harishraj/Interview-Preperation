# Paint House 
### Source
1. [leetcode](https://leetcode.com/problems/paint-house/)

> There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.
>
> The cost of painting each house with a certain color is represented by a n x 3 cost matrix. For example, costs[0][0] is the cost of painting house 0 with color red; costs[1][2] is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.
>
> Note:
>
> All costs are positive integers.

## Method 1: DFS (TLE)
### Analysis
The most straightforward method is to use DFS to search all possible combination. 

### Complexity
Time: O(3^n)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  private static final int NUM_COLOR = 3;
  private int minCost;

  public int minCost(int[][] costs) {
    minCost = Integer.MAX_VALUE;
    dfs(costs, 0 /* index */, 0 /* totalCost */, -1 /* prevColor */);
    return minCost;
  }

  private void dfs(int[][] costs, int index, int totalCost, int prevColor) {
    if (index == costs.length) {
      minCost = Math.min(minCost, totalCost);
      return ;
    }
    for (int color = 0; color < NUM_COLOR; color++) {
      if (color == prevColor) {
        continue;
      }
      dfs(costs, index + 1, totalCost + costs[index][color], color);
    }
  }
}
```

## Method 2: DP
### Analysis

### Complexity
Time: O(n)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  // It is easy to change to k different colors.
  private static final int NUM_COLOR = 3;

  public int minCost(int[][] costs) {
    if (costs == null || costs.length == 0) {
      return 0;
    }
    int[][] minCosts = new int[2][NUM_COLOR];
    for (int i = 0; i < NUM_COLOR; i++) {
      minCosts[0][i] = costs[0][i];
    }

    for (int i = 1; i < costs.length; i++) {
      for (int color = 0; color < NUM_COLOR; color++) {
        minCosts[i % 2][color] = getMinCost(minCosts, costs[i][color], (i - 1) % 2, color);
      }
    }
    return getMin(minCosts, (costs.length - 1) % 2);
  }

  private int getMinCost(int[][] minCosts, int cost, int prevIndex, int color) {
    int minCost = Integer.MAX_VALUE;
    for (int prevColor = 0; prevColor < NUM_COLOR; prevColor++) {
      if (prevColor != color) {
        minCost = Math.min(minCost, minCosts[prevIndex][prevColor] + cost);
      }
    }
    return minCost;
  }

  private int getMin(int[][] minCosts, int index) {
    int minCost = Integer.MAX_VALUE;
    for (int color = 0; color < NUM_COLOR; color++) {
      minCost = Math.min(minCost, minCosts[index][color]);
    }
    return minCost;
  }
}
```
