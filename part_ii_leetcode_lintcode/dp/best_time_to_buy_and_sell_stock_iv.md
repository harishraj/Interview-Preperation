# Best Time to Buy and Sell Stock IV
### Source
1. [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-iv/)
2. [leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

> Say you have an array for which the ith element is the price of a given stock on day i.
> 
> Design an algorithm to find the maximum profit. You may complete at most k transactions.
>
> Note:
>
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Analysis
If k >= n/2, we can have transactions any time, it can be solved by O(n) as Best Time to Buy and Sell Stock II. Otherwise, we can do it in DP.

Use dp[k][i+1] to represent the max profit of using [0,i] data and k transactions.

So we have:

```java
dp[k][i+1] = max(dp[k-1][i+1], dp[k][i], max(dp[k-1][j] + prices[i] - prices[j]))  {0 <= j < i}
           = max(dp[k-1][i+1], dp[k][i], prices[i] + max(dp[k-1][j] - prices[j])) {0 <= j < i}
```
The second equation is a huge optimization to find max(dp[k - 1][j] + prices[i] - prices[j]) {0 <= j < i}. Orignally, we we need a forloop to iterate j from 0 to i. However, since j range from 0 to i, we can simply compute max(dp[k-1][j] - prices[j]) {0 <= j < i} while iterating on i.

We optimize 
```java
for (int i = 0; i < prices.length; ++i) {
  profit[t][i + 1] = Math.max(profit[t][i], profit[t - 1][i + 1]);
  for (int j = 0; j < i; ++j) {
    profit[t][i + 1]= Math.max(
        profit[t][i + 1], 
        profit[t][j + 1] + prices[i] - prices[j]);
  }
}
```
to 
```java
for (int i = 0; i < prices.length; ++i) {
  profit[t][i + 1] = Math.max(
      Math.max(profit[t][i], profit[t - 1][i + 1]),
      prices[i] + curMax);
  curMax = Math.max(curMax, profit[t - 1][i] - prices[i]);
}
```

We can further optimize the space complexity since profit[t][i] only depends on profit[t - 1][i] and profit[t - 1][i - 1], we can use two variables to store this.

### Complexity
Time: O(kn)

Space: O(n)

### Code
#### Java 
##### Version 1: O(kn) space
```java
public class Solution {
  public int maxProfit(int k, int[] prices) {
    if (k >= prices.length / 2) {
      return quickSolve(prices);
    }

    int[][] profit = new int[k + 1][prices.length + 1];
    for (int t = 1; t <= k; ++t) {
      int curMax = Integer.MIN_VALUE;
      for (int i = 0; i < prices.length; ++i) {
        profit[t][i + 1] = Math.max(
            Math.max(profit[t][i], profit[t - 1][i + 1]),
            prices[i] + curMax);
        curMax = Math.max(curMax, profit[t - 1][i] - prices[i]);
      }
    }
    return profit[k][prices.length];
  }

  private int quickSolve(int[] prices) {
    int profit = 0;
    for (int i = 1; i < prices.length; i++) {
      if (prices[i] > prices[i - 1]) {
        profit += prices[i] - prices[i - 1];
      }
    }

    return profit;
  }
}
```

##### Version 2: O(n) space
```java
public class Solution {
  public int maxProfit(int k, int[] prices) {
    if (k >= prices.length / 2) {
      return quickSolve(prices);
    }

    int[] profit = new int[prices.length + 1];
    for (int t = 1; t <= k; ++t) {
      int curMax = Integer.MIN_VALUE;
      int lastPrev = profit[0];
      int prev = profit[0];
      for (int i = 0; i < prices.length; ++i) {
        lastPrev = profit[i + 1];
        profit[i + 1] = Math.max(
            Math.max(prev, profit[i + 1]),
            prices[i] + curMax);
        curMax = Math.max(curMax, lastPrev - prices[i]);
        prev = profit[i + 1];
      }
    }
    return profit[prices.length];
  }

  private int quickSolve(int[] prices) {
    int profit = 0;
    for (int i = 1; i < prices.length; i++) {
      if (prices[i] > prices[i - 1]) {
        profit += prices[i] - prices[i - 1];
      }
    }
    return profit;
  }
}
```


