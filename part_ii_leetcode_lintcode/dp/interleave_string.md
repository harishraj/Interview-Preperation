# Interleave String
> Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.
>
> For example,
>
> Given:
>
> s1 = "aabcc",
>
> s2 = "dbbca",
>
> When s3 = "aadbbcbcac", return true.
>
> When s3 = "aadbbbaccc", return false.

## Method 1: DFS (Got TLE)
### Analysis
Use DFS to search all permutation possibilities.

### Complexity
Time: O((m + n)!)

### Code
#### Java
```java
public class Solution {
  public boolean isInterleave(String s1, String s2, String s3) {
    assert s1 != null && s2 != null && s3 != null;
    if (s1.length() + s2.length() != s3.length()) {
      return false;
    }
    return isInterleave(s1, s2, s3, 0, 0);
  }

  private boolean isInterleave(String s1, String s2, String s3, int index1, int index2) {
    if (index1 + index2 == s3.length()) {
      return true;
    }
    if (index1 < s1.length() && s1.charAt(index1) == s3.charAt(index1 + index2)) {
      if (isInterleave(s1, s2, s3, index1 + 1, index2)) {
        return true;
      }
    }
    if (index2 < s2.length() && s2.charAt(index2) == s3.charAt(index1 + index2)) {
      if (isInterleave(s1, s2, s3, index1, index2 + 1)) {
        return true;
      }
    }
    return false;
  }
}
```

## Method 2: DP
### Analysis
The problem can be solved using DP. dp[i][j] represents if the first i characters of s1 and the first j characters of s2 can form a interleaving of s3's first i + j characters.

Update equation is:
```dp[i][j] = (dp[i - 1][j] && s1[i]==s3[i+j]) || (dp[i][j - 1] && s2[j]==s3[i+j])```

### Complexity
Time: O(len1 * len2)

Space: O(len1 * len2)

### Code
#### Java Version 1
public class Solution {
  public boolean isInterleave(String s1, String s2, String s3) {
    if (s1 == null || s2 == null || s3 == null) {
      return false;
    } else if (s1.length() + s2.length() != s3.length()) {
      return false;
    } 
    int len1 = s1.length();
    int len2 = s2.length();
    boolean[][] isValid = new boolean[len1 + 1][len2 + 1];
    isValid[0][0] = true;
    for (int i = 0; i < len1; i++) {
      isValid[i + 1][0] = isValid[i][0] && s1.charAt(i) == s3.charAt(i);
      if (!isValid[i + 1][0]) {
        break;
      }
    }
    for (int i = 0; i < len2; i++) {
      isValid[0][i + 1] = isValid[0][i] && s2.charAt(i) == s3.charAt(i);
      if (!isValid[0][i + 1]) {
        break;
      }
    }
    for (int i = 0; i < len1; i++) {
      for (int j = 0; j < len2; j++) {
        isValid[i + 1][j + 1] = (isValid[i][j + 1] && s1.charAt(i) == s3.charAt(i + j + 1))
          || (isValid[i + 1][j] && s2.charAt(j) == s3.charAt(i + j + 1));
      }
    }
    return isValid[len1][len2];
  }
}

#### Java Version 2 (Less code)
```java
public class Solution {
  public boolean isInterleave(String s1, String s2, String s3) {
    if (s1 == null || s2 == null || s3 == null) {
      return false;
    } else if (s1.length() + s2.length() != s3.length()) {
      return false;
    } 
    int len1 = s1.length();
    int len2 = s2.length();
    boolean[][] isValid = new boolean[len1 + 1][len2 + 1];
    isValid[0][0] = true;
    for (int i = 0; i <= len1; i++) {
      for (int j = 0; j <= len2; j++) {
        if (i > 0 && s1.charAt(i - 1) == s3.charAt(i + j - 1)) {
          isValid[i][j] = isValid[i - 1][j];
        }
        if (j > 0 && s2.charAt(j - 1) == s3.charAt(i + j - 1)) {
          isValid[i][j] |= isValid[i][j - 1];
        }
      }
    }
    return isValid[len1][len2];
  }
}
```

