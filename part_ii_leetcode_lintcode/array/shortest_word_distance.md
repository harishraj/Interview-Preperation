# Shortest Word Distance
### Source
1. [leetcode](https://leetcode.com/problems/shortest-word-distance/)

> Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.
> 
> For example,
>
> Assume that words = ["practice", "makes", "perfect", "coding", "makes"].
> 
> Given word1 = “coding”, word2 = “practice”, return 3.
>
> Given word1 = "makes", word2 = "coding", return 1.
> 
> Note:
>
> You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

### Analysis
Record the index when word1 or word2 last appeared and update the min distance. 
### Complexity
Time: O(n)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int shortestDistance(String[] words, String word1, String word2) {
    if (word1.equals(word2)) {
      return 0;
    }
    int index1 = -1;
    int index2 = -1;
    int minDistance = Integer.MAX_VALUE;
    for (int i = 0; i < words.length; i++) {
      if (words[i].equals(word1)) {
        index1 = i;
      } else if (words[i].equals(word2)) {
        index2 = i;
      }
      if (index1 != -1 && index2 != -1) {
        minDistance = Math.min(minDistance, Math.abs(index1 - index2));
      }
    }
    return minDistance;
  }
}
```
