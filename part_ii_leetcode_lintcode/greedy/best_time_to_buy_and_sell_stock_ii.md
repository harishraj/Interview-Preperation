# Best Time to Buy and Sell Stock II
### Source
1. [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-ii/)
2. [leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Analysis
This problem can be solved using Greedy algorithm. Since we are allowed to make as many transaction as possible, as long as prices[i] is greater than prices[i - 1], we can make a transaction and we will make profit. 

### Complexity
Time: O(N)

Space: O(1)

### Code
```java
class Solution {
  public int maxProfit(int[] prices) {
    if (prices == null) {
      return 0;
    }
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

