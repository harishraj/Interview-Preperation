# Shortest Word Distance III 
### Source 
1. [leetcode](https://leetcode.com/problems/shortest-word-distance-iii/)

> This is a follow up of Shortest Word Distance. The only difference is now word1 could be the same as word2.
> 
> Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.
> 
> word1 and word2 may be the same and they represent two individual words in the list.
> 
> For example,
>
> Assume that words = ["practice", "makes", "perfect", "coding", "makes"].
> 
> Given word1 = “makes”, word2 = “coding”, return 1.
>
> Given word1 = "makes", word2 = "makes", return 3.
> 
> Note:
>
> You may assume word1 and word2 are both in the list.

### Analysis
Almost the same as [Shortest Word Distance](shortest_word_distance.md) except that we have to deal with the special case that word1 == word2.

### Complexity
Time: O(n)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public int shortestWordDistance(String[] words, String word1, String word2) {
    int index1 = -1;
    int index2 = -1;
    int minDistance = Integer.MAX_VALUE;
    for (int i = 0; i < words.length; i++) {
      if (words[i].equals(word1)) {
        if (index1 != -1 && word1.equals(word2)) {
          minDistance = Math.min(minDistance, Math.abs(i - index1));
        }
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
