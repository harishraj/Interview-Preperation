# Shortest Word Distance II 
### Source 
1. [leetcode](https://leetcode.com/problems/shortest-word-distance-ii/)

> This is a follow up of Shortest Word Distance. The only difference is now you are given the list of words and your method will be called repeatedly many times with different parameters. How would you optimize it?
> 
> Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list.
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
> You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

### Analysis
The question is different from [Shortest Word Distance](../array/shortest_word_distance.md) in that it will be query many times. So a better approach is to preprocess the words list by storing all indexes of a certain word into a hash table.

### Complexity
Time: preprocess: O(n), query: O(n)

Space: preprocess: O(n), query: O(1)

### Code
```java
public class WordDistance {
  private Map<String, List<Integer>> wordIndexMap;
  public WordDistance(String[] words) {
    wordIndexMap = new HashMap<>();
    for (int i = 0; i < words.length; i++) {
      List<Integer> indexList;
      if (wordIndexMap.containsKey(words[i])) {
        indexList = wordIndexMap.get(words[i]);
      } else {
        indexList = new ArrayList<>();
      }
      indexList.add(i);
      wordIndexMap.put(words[i], indexList);
    }
  }

  public int shortest(String word1, String word2) {
    if (word1.equals(word2)) {
      return 0;
    }
    assert (wordIndexMap.containsKey(word1) && wordIndexMap.containsKey(word2));
    int minDistance = Integer.MAX_VALUE;
    List<Integer> indexList1 = wordIndexMap.get(word1);
    List<Integer> indexList2 = wordIndexMap.get(word2);
    int index1 = 0;
    int index2 = 0;
    while (index1 < indexList1.size() && index2 < indexList2.size()) {
      minDistance = Math.min(minDistance, 
          Math.abs(indexList1.get(index1) - indexList2.get(index2)));
      if (indexList1.get(index1) > indexList2.get(index2)) {
        index2++;
      } else {
        index1++;
      }
    }
    return minDistance;
  }
}

// Your WordDistance object will be instantiated and called as such:
// WordDistance wordDistance = new WordDistance(words);
// wordDistance.shortest("word1", "word2");
// wordDistance.shortest("anotherWord1", "anotherWord2");
```
