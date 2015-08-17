#  Merge Intervals
> Given a collection of intervals, merge all overlapping intervals.
>
> For example,
>
> Given [1,3],[2,6],[8,10],[15,18],
>
> return [1,6],[8,10],[15,18].

### Problem
1. how two deal with [1, 2], [2, 4]

### Analysis
1. Sort the intervals according to start point. (It is optional to sort according to end point if start points tie)
2. Use two variable to record the merged intervals start and end. Initially, *start = interval0.start*, *end = interval0.end*. 
3. If new interval's start is greater than merged interval's end, then there is no overlap, we can add the merged interval into result and set the new interval as the merged interval. 
4. Otherwise, new interval is overlap with merge interval. We need to expand the merge interval by *end = max(end, interval.end)*

### Complexity
Time: O(NlgN), sorting

Space: O(1)

### Code
#### Java
```java
public class Solution {
  private static final Comparator<Interval> INTERVAL_COMPARATOR = 
      new Comparator<Interval>() {
        @Override
        public int compare(Interval i1, Interval i2) {
          return Integer.compare(i1.start, i2.start);
        }
      };

  public List<Interval> merge(List<Interval> intervals) {
    List<Interval> result = new ArrayList<>();
    if (intervals == null || intervals.size() == 0) {
      return result;
    }

    // Sort according to the start.
    Collections.sort(intervals, INTERVAL_COMPARATOR);
    
    int start = intervals.get(0).start;
    int end = intervals.get(0).end;
    for (int i = 1; i < intervals.size(); ++i) {
      Interval interval = intervals.get(i);
      if (interval.start <= end) {
        end = Math.max(end, interval.end);
      } else {
        result.add(new Interval(start, end));
        start = interval.start;
        end = interval.end;
      }
    }
    result.add(new Interval(start, end));
    return result;
  }
}
```

