# Coins in a Line
> There are n coins in a line. Two players take turns to take one or two coins from right side until there are no more coins left. The player who take the last coin wins.
>
> Could you please decide the first play will win or lose?
>
> Example
>
> n = 1, return true.
>
> n = 2, return true.
> 
> n = 3, return false.
>
> n = 4, return true.
> 
> n = 5, return true.

## Method 1: Divide and Conquer
### Analysis
We can break down the question into that after the first player choose, there exist one case that cause the other player to lose. We can represent the condition that the opponent will always win as `opponent = willWin(n - 1) && willWin(n - 2)`. So we can represent that the opponent may lose as `!(willWin(n - 1) && willWin(n - 2))`

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  Map<Integer, Boolean> cache = new HashMap<>();
  public boolean firstWillWin(int n) {
    if (n == 1 || n == 2) {
      return true;
    } else if (cache.containsKey(n)) {
      return cache.get(n);
    }
    boolean opponentWin = firstWillWin(n - 1) && firstWillWin(n - 2);
    cache.put(n, !opponentWin);
    return !opponentWin;
  }
}
```

## Method 2: DP
### Analysis
Since we can use Divide and Conquer with Memo, we can transfer the idea to using DP, with time complexity O(N).

### Code
#### Java Version 1: Space O(N)
```java
public class Solution {
  public boolean firstWillWin(int n) {
    if (n == 0) {
      return false;
    }
    boolean[] isWin = new boolean[n + 1];
    isWin[0] = false;
    isWin[1] = true;
    for (int i = 2; i <= n; i++) {
      isWin[i] = !(isWin[i - 2] && isWin[i - 1]);
    }
    return isWin[n];
  }
}
```

#### Java Version 2: Space O(1)
```java
public class Solution {
  public boolean firstWillWin(int n) {
    if (n == 0) {
      return false;
    }
    boolean prev2 = false;  // n - 2
    boolean prev1 = true;   // n - 1
    boolean isWin = true;
    for (int i = 2; i <= n; i++) {
      isWin = !(prev2 && prev1);
      prev2 = prev1;
      prev1 = isWin;
    }
    return isWin;
  }
}
```

