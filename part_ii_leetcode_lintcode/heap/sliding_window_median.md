# Sliding Window Median
> Given an array of n integer, and a moving window(size k), move the window at each iteration from the start of the array, find the median of the element inside the window at each moving. (If there are even numbers in the array, return the N/2-th number after sorting the element in the window. )
>
> Example
>
> For array [1,2,7,8,5], moving window size k = 3. return [2,7,7]
>
> At first the window is at the start of the array like this
>
> [ | 1,2,7 | ,8,5 ] , return the median 2;
>
> then the window move one step forward.
>
> [1, | 2,7,8 | ,5], return the median 7;
>
> then the window move one step forward again.
>
> [1,2, | 7,8,5 | ], return the median 7;

## Method 1: Two Heap
### Analysis
Use a max heap to store elements smaller or equal to median, and use a min heap to store elements greater than median.

It is important to balance two heaps. The number of elements in two heaps should be equal, or max heap should has one more element if k is odd.

### Complexity
Time: O(Nlgk), it is actually O(N*k*lgk), since Java PriorityQueue.remove(Object o), needs to do a linear scan to find the element. It is possible to implement heap our self and use a hashmap to avoid linear scanning.

Space: O(k)

### Code
#### Java
```java
public class Solution {
  public ArrayList<Integer> medianSlidingWindow(int[] nums, int k) {
    ArrayList<Integer> result = new ArrayList<>();
    if (nums == null || nums.length == 0 || k == 0) {
      return result;
    }
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(k, Collections.reverseOrder());
    PriorityQueue<Integer> minHeap = new PriorityQueue<>(k);
    for (int i = 0; i < nums.length; i++) {
      // add new element into one of the heap
      if (maxHeap.isEmpty() || nums[i] < maxHeap.peek()) {
        maxHeap.offer(nums[i]);
      } else {
        minHeap.offer(nums[i]);
      }
      // remove element outside of window
      if (i >= k) {
        if (nums[i - k] <= maxHeap.peek()) {
          maxHeap.remove(nums[i - k]);
        } else {
          minHeap.remove(nums[i - k]);
        }
      }
      // balance two heaps, make sure maxHeap contains one more element if k is odd.
      while (minHeap.size() >= maxHeap.size() + 1) {  
        maxHeap.offer(minHeap.poll());
      }
      while (maxHeap.size() > minHeap.size() + 1) {
        minHeap.offer(maxHeap.poll());
      }
      // add result
      if (i >= k - 1) {
        result.add(maxHeap.peek());
      }
    }
    return result;
  }
}
```
