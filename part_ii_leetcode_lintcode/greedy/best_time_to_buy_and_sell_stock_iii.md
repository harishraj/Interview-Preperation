# Best Time to Buy and Sell Stock III
### Source
1. [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-iii/)
2. [leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> Design an algorithm to find the maximum profit. You may complete at most **two** transactions.
>
> Note:
> 
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Analysis
In [Best Time to Buy and Sell Stock](best_time_to_buy_and_sell_stock.md), we go from the from to back. Every time we update min price seen so far and record the maximum profit from day 0 to day i. 

We can do this from back to front. We use a variable to record the ma price seen so far. For every entry, we record the maximum profit from day i to the last day. 

If we combine these two processes, at day i, we know the maximum profit from the first day to day i, and we know the maximum profit from day i to the last day. In this way, we can get the maximum profit if we take day i as the splitting point. If we get the maximum value for every possible splitting point, we will get the final result. 

e.g.

[6, 1, 3, 2, 4, 7]

1. Scan from front to back. i = 0, profit1[0] = 0, minPrice = 6; i = 1, profit1[1] = 0, minPrice = 1; i = 2, profit1[2] = 2, minPrice = 1; i = 3, profit1[3] = max(profit1[2], prices[3]-minPrice) = 2, minPrice = 1; i = 4, profit1[4] = 3, minPrice = 1; i = 5, profit1[5] = 6, minPrice = 1.
2. Scan from back to front. i = 5, profit2[5] = 0, maxPrice = 7; i = 4, profit2[4] = 3, maxPrice = 7; i = 3, profit2[3] = 5, maxPrice = 7; i = 2, profit2[2] = 5, maxPrice = 7; i = 1, profit2[1] = 6, maxPrice = 7; i = 0, profit2[0] = 6, maxPrice = 7;
3. For every spillting point, we find that when i = 2 or i = 3, we will get the maximum result 7. The transactions are {buy 1, sell 3} and {buy 2, sell 7} 

### Complexity
Time: O(n)

Space: O(1)

### Code
#### Java
```java
class Solution {
  public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) {
      return 0;
    }
    // scan from left to right.
    int[] profit = new int[prices.length];
    int minPrice = prices[0];
    for (int i = 1; i < prices.length; i++) {
      profit[i] = Math.max(profit[i - 1], prices[i] - minPrice);
      minPrice = Math.min(minPrice, prices[i]);
    }
    // scan from right to left.
    int maxProfit = profit[prices.length - 1];
    int maxPrice = prices[prices.length - 1];
    int curProfit = 0;
    for (int i = prices.length - 2; i >= 0; i--) {
      curProfit = Math.max(curProfit, maxPrice - prices[i]);
      maxPrice = Math.max(maxPrice, prices[i]);
      maxProfit = Math.max(maxProfit, curProfit + profit[i]);
    }
    return maxProfit;
  }
}
```
