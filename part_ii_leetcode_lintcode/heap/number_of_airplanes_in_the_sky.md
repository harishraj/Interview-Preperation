# Number of Airplanes in the Sky
> Given an interval list which are flying and landing time of the flight. How many airplanes are on the sky at most?
>
> Example
>
> For interval list [[1,10],[2,3],[5,8],[4,7]], return 3
>
> Note
>
> If landing and flying happens at the same time, we consider landing should happen at first.

## Method 1: Min Heap
### Analysis
The problem can be abstracted to how many intervals can a vertical line cross at the same time. 

We can sort all intervals according to start time and add end time into a heap. Before adding, remove all end time that is smaller than the current start time. And the maximum number of elements in the min heap is the result.

### Complexity
Time: O(nlgn)

Space: O(n)

### Code
#### Java
```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */

class Solution {
  private static final Comparator<Interval> INTERVAL_COMPARATOR = 
      new Comparator<Interval>() {
    @Override
    public int compare(Interval i1, Interval i2) {
      return Integer.compare(i1.start, i2.start);
    }
  };

  /**
   * @param intervals: An interval array
   * @return: Count of airplanes are in the sky.
   */
  public int countOfAirplanes(List<Interval> airplanes) { 
    if (airplanes == null || airplanes.size() == 0) {
      return 0;
    }
    Collections.sort(airplanes, INTERVAL_COMPARATOR);
    Queue<Integer> minHeap = new PriorityQueue<>();
    int maxCount = 0;
    for (Interval interval : airplanes) {
      while (!minHeap.isEmpty() && interval.start >= minHeap.peek()) {
        minHeap.poll();
      }
      minHeap.offer(interval.end);
      maxCount = Math.max(maxCount, minHeap.size());
    }
    return maxCount;
  }
}
```
