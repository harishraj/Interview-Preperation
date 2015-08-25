# Candy
### Source
1. [lintcode](http://www.lintcode.com/en/problem/candy/)
2. [leetcode](https://leetcode.com/problems/candy/)

> There are N children standing in a line. Each child is assigned a rating value.
>
> You are giving candies to these children subjected to the following requirements:
>
> * Each child must have at least one candy.
> * Children with a higher rating get more candies than their neighbors.
>
> What is the minimum candies you must give?

### Analysis 
Initially, all children should have one candy, because every child must have at least one candy. 

Then we need two passes.

* Scan from left to right. If the child has a rating greater than its left neighbor, then set its candy number as its left neighbor's candy number + 1. 
* Scan from right to left. If the child has a rating greater than its right neighbor, then set its candy number as Max(its own candy num, its right neighbor's candy num + 1).

example 1:

    ratings: 3 2 5 4 1 3 6
    candies: 1 1 2 1 1 2 3 (first pass)
    candies: 2 1 3 2 1 2 3 (second pass)

example 2:

    ratings: 1 2 3 2 2 1 1
    candies: 1 2 3 1 1 1 1
    candies: 1 2 3 1 2 1 1

Noticed that when we are doing the second pass, we find that ratings[2]=3 is greater than its right neighbor ratings[3]=2. Then we set candies[2] to Max(candies[2], candies[3] + 1) instead of just setting candies[i] = candies[3] + 1 = 2, which will lead to a wrong answer.

Also we should notice that:

for ratings: 2 2 1 1, we set the candies to 1 2 1 1 instead of 2 2 1 1. Because we do not need to guarantee that children with equal ratings should have the same candies. 

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public int candy(int[] ratings) {
    if (ratings == null || ratings.length == 0) {
      return 0;
    }
    // scan from left to right
    int[] leftCandy = new int[ratings.length];
    leftCandy[0] = 1;
    for (int i = 1; i < ratings.length; i++) {
      if (ratings[i] > ratings[i - 1]) {
        leftCandy[i] = leftCandy[i - 1] + 1;
      } else {
        leftCandy[i] = 1;
      }
    }
    // scan from right to left
    int rightCandy = 1;
    int numCandies = 0;
    for (int i = ratings.length - 1; i >= 0; i--) {
      if (i < ratings.length - 1 && ratings[i] > ratings[i + 1]) {
        rightCandy += 1;
      } else {
        rightCandy = 1;
      }
      numCandies += Math.max(leftCandy[i], rightCandy);
    }
    return numCandies;
  }
}
```

