# 4 Sum
### Source
1. [lintcode](http://www.lintcode.com/en/problem/4-sum/)
2. [leetcode](https://leetcode.com/problems/4sum/)

> Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.
>
> Note:
>
> Elements in a quadruplet (a,b,c,d) must be in non-descending order. (ie, a ≤ b ≤ c ≤ d)
The solution set must not contain duplicate quadruplets.
>
> For example, given array S = {1 0 -1 0 -2 2}, and target = 0.
>
> A solution set is:
>
>     (-1,  0, 0, 1)
>     (-2, -1, 1, 2)
>     (-2,  0, 0, 2)

## Method 1: Two Pointers
### Analysis
We can use the same method as in 3Sum.

### Complexity
Time: O(n^3)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  public List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    if (nums == null || nums.length < 4) {
      return result;
    }
    Arrays.sort(nums);
    int len = nums.length;
    for (int i = 0; i < len - 3; ++i) {
      if (i != 0 && nums[i] == nums[i - 1]) {
        continue;
      }
      for (int j = i + 1; j < len - 2; ++j) {
        if (j != i + 1 && nums[j] == nums[j - 1]) {
          continue;
        }
        int start = j + 1;
        int end = len - 1;
        while (start < end) {
          if (start != j + 1 && nums[start] == nums[start - 1]) {
            ++start;
            continue;
          }
          if (end != len - 1 && nums[end] == nums[end + 1]) {
            --end;
            continue;
          }
          int sum = nums[i] + nums[j] + nums[start] + nums[end];
          if (sum == target) {
            result.add(Arrays.asList(nums[i], nums[j], nums[start++], nums[end--]));
          } else if (sum < target) {
            ++start;
          } else {
            --end;
          }
        }
      }
    }
    return result;
  }
}
```

## Method 2: 2Sum + 2Sum
### Analysis
The second method is to preprocess the array and precompute the sum of two numbers. Then we can treat the problem as 2 Sum on the sum of two numbers. The time complexity for this algorithm is O(N^2).

### Complexity
Time: O(n^2)

Space: O(n^2)

### Code
#### Java
```java
public class Solution {
  class Pair {
    int i;
    int j;
    public Pair(int i, int j) {
      this.i = i;
      this.j = j;
    }
  }
  
  class Answer {
    List<Integer> num;
    String str;
    public Answer (int num1, int num2, int num3, int num4) {
      num = new ArrayList<Integer>(
          Arrays.asList(num1, num2, num3, num4));
      Collections.sort(num);
      str = buildString();
    }
    
    private String buildString() {
      StringBuilder sb = new StringBuilder();
      for (int i = 0; i < num.size(); ++i) {
        sb.append(num.get(i));
        sb.append("_");
      }
      return sb.toString();
    }
    
    @Override
    public int hashCode() {
      return str.hashCode();
    }
    
    @Override
    public boolean equals(Object obj) {
      if (obj == this) {
        return true;
      } else if (obj instanceof Answer) {
        Answer that = (Answer)obj;
        for (int i = 0; i < num.size(); ++i) {
          // use equals instead of ==, because Integer is object
          if (!this.num.get(i).equals(that.num.get(i))) {
            return false;
          }
        }
        return true;
      }
      return false;
    }
  }

  public List<List<Integer>> fourSum(int[] num, int target) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    Set<Answer> resultSet = new HashSet<Answer>();
    if (num == null || num.length < 4) {
      return result;
    }
    int len = num.length;
    Map<Integer, List<Pair>> map = 
        new HashMap<Integer, List<Pair>>();
    // preprocess to get sum of all pairs, and store in a map
    for (int i = 0; i < len; ++i) {
      for (int j = i + 1; j < len; ++j) {
        Pair p = new Pair(i, j);
        int sum = num[i] + num[j];
        List<Pair> pairs = null;
        if (map.containsKey(sum)) {
          pairs = map.get(sum);
        } else {
          pairs = new ArrayList<Pair>();
        }
        pairs.add(p);
        map.put(sum, pairs);
      }
    }

    for (Integer sum: map.keySet()) {
      List<Pair> pairs1 = map.get(sum);
      int remaining = target - sum;
      if (map.containsKey(remaining)) {
        List<Pair> pairs2 = map.get(remaining);
        for (Pair p1: pairs1) {
          for (Pair p2: pairs2) {
            if (p1.i != p2.i && p1.j != p2.j && p1.i != p2.j && p1.j != p2.i) {
              Answer answer = new Answer(num[p1.i], num[p1.j], num[p2.i], num[p2.j]);
              resultSet.add(answer);
            }
          }
        }
      }
    }
    for (Answer answer: resultSet) {
      result.add(answer.num);
    }
    return result;
  }
}
```

