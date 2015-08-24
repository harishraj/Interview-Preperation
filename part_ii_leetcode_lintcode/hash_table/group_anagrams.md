# Group Anagrams
### Source 
1. [leetcode/Group Anagrams](https://leetcode.com/problems/anagrams/)

> Given an array of strings, group anagrams together.
>
> For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"], 
>
> Return:
>
>      [
>        ["ate", "eat","tea"],
>        ["nat","tan"],
>        ["bat"]
>      ]
>
> Note:
>
> For the return value, each inner list's elements must follow the lexicographic order.
>
> All inputs will be in lower-case.

## Version 1: Sorting
### Analysis
Sort the string as key. The time to sort a string is O(klogk), where k is the length of the string. When most strings are short, it is efficient. One way to do this is to use a List to store all anagrams related to a key. However, it is actually not needed. We can use add the anagram to the result on the fly with some trick.(See Version 2)

### Complexity
Time: O(n * klgk)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();
    for (String str : strs) {
      char[] charArr = str.toCharArray();
      Arrays.sort(charArr);
      String sortedStr = new String(charArr);
      List<String> anagramList;
      if (map.containsKey(sortedStr)) {
        anagramList = map.get(sortedStr);
      } else {
        anagramList = new ArrayList<>();
      }
      anagramList.add(str);
      map.put(sortedStr, anagramList);
    }
    List<List<String>> result = new ArrayList<>();
    for (List<String> anagramList : map.values()) {
      Collections.sort(anagramList);
      result.add(anagramList);
    }
    return result;
  }
}
```

## Version 2: Count Sort + String compression
### Analysis
Instead of using regular sort, use count sort to get the number of occurrences of each character. And represent string like "acbacab" to "a3b2c2" as the key.

### Complexity
Time: O(k * n)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();
    for (String str : strs) {
      String key = getKey(str);
      List<String> anagramList;
      if (map.containsKey(key)) {
        anagramList = map.get(key);
      } else {
        anagramList = new ArrayList<>();
      }
      anagramList.add(str);
      map.put(key, anagramList);
    }
    List<List<String>> result = new ArrayList<>();
    for (List<String> anagramList : map.values()) {
      Collections.sort(anagramList);
      result.add(anagramList);
    }
    return result;
  }

  private String getKey(String str) {
    int[] count = new int[26];
    for (char c: str.toCharArray()) {
      ++count[c - 'a'];
    }
    StringBuilder sb = new StringBuilder();
    for (char c = 'a'; c <= 'z'; c++) {
      if (count[c - 'a'] > 0) {
        sb.append(c);
        sb.append(count[c - 'a']);
      }
    }
    return sb.toString();
  }
}
```

## Version 3: Count Sort + Hash
### Analysis
Use a hash value as the key. However, it cannot work if there is a conflict. 

### Complexity
Time: O(n * k)

Space: O(n)

### Code
#### Java
```java
public class Solution {
  public List<List<String>> groupAnagrams(String[] strs) {
    Map<Integer, List<String>> map = new HashMap<>();
    for (String str : strs) {
      int hashCode = computeHashCode(str);
      List<String> anagramList;
      if (map.containsKey(hashCode)) {
        anagramList = map.get(hashCode);
      } else {
        anagramList = new ArrayList<>();
      }
      anagramList.add(str);
      map.put(hashCode, anagramList);
    }
    List<List<String>> result = new ArrayList<>();
    for (List<String> anagramList : map.values()) {
      Collections.sort(anagramList);
      result.add(anagramList);
    }
    return result;
  }

  private int computeHashCode (String str) {
    int[] count = new int[26];
    for (char c: str.toCharArray()) {
      ++count[c - 'a'];
    }
    int hashCode = 17;
    for (int val: count) {
      hashCode = hashCode * 31 + val;
    }
    return hashCode;
  }
}
```
