# Best Time to Buy and Sell Stock 
### Source
1. [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock/)
2. [leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

### Analysis
The problem can be solved using greedy algorithm. We use a vairable to keep track of the min price seen so far, and update the max profit using the new price.

### Corner Cases
1. Array Length <= 1.

### Complexity
Time: O(n)

Space: O(1)

### Code
```java
public class Solution {
  public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) {
      return 0;
    }

    int minPrice = prices[0];
    int maxProfit = 0;
    for (int i = 1; i < prices.length; ++i) {
      maxProfit = Math.max(maxProfit, prices[i] - minPrice);
      minPrice = Math.min(minPrice, prices[i]);
    }
    return maxProfit;
  }
}
```

