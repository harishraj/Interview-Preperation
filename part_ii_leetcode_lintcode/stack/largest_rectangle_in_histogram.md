# Largest Rectangle in Histogram
> Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.
>
> ![image](http://www.leetcode.com/wp-content/uploads/2012/04/histogram.png)
>
> Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].
>
> ![image](http://www.leetcode.com/wp-content/uploads/2012/04/histogram_area.png)
>
> The largest rectangle is shown in the shaded area, which has area = 10 unit.
>
> For example,
>
> Given height = [2,1,5,6,2,3],
>
> return 10.

## Method 1
The most straightforward method is to enumerate all possible pair of range, and then multiply the range with the min height in this range, whose complexity is O(N^3). 

If we fix the start point and enumerate all end point, we can maintain the min height seen so far, which can improves the time complexity to O(N^2).

## Method 2
Here is an analysis from [geeksforgeeks](http://www.geeksforgeeks.org/largest-rectangle-under-histogram/)

The basic idea is that we use a **stack** to store a list of **non-decreasing** histogram's **index**. Each time the new bar is less than the top element in the stack, we know that we cannot find contiguous bars whose height is stack[top] across the new bar. (Since the new element is small than stack[top]). So the area of the contiguous bars whose height is stack[top] is `height[stack[top]] * (new_index - (stack[top - 1] + 1))` where `stack[top - 1] + 1` is the left bound of the contiguous bars. Why? Since we may have already pop out bars large than stack[top] from the stack. And stack[top - 1] is the index of the first element that is smaller than stack[top] that has appeared before. All elements between stack[top - 1] and stack[top]\(If exist) are larger than stack[top]. (They have been popped out of stack)

Take the example above, when we reach the last element 3, the elements' **index** in the stack is now {1, 4, 5}. We now need to compute contiguous bars whose height is 3. We pop 5 from the stack. The area is 3 * (6 - (4 + 1)) according to the `height[stack[top]] * (new_index - (stack[top - 1] + 1))`. Let's take a step further.

The stack now is {1, 4}. When we pop element 2 whose index is 4 from the stack, the area now should be 2 * (6 - (1 + 1)) = 2 * 4 = 8. See what happened? The left bound of the contigous bars whose height is 2 should be index = 1 + 1 = 2, which is the bar with height 5. The right bound is the last bar with height 3. Since any elements between the new elements and the top element in the stack is also greater than the top element(otherwise it will not be pushed into stack).

### Complexity
Time: O(N)

Space: O(N)

### Code
#### Java
```java
public class Solution {
  public int largestRectangleArea(int[] height) {
    if (height == null || height.length == 0) {
      return 0;
    }

    int maxArea = 0;
    Stack<Integer> indexStack = new Stack<>();
    indexStack.push(-1);
    for (int i = 0; i < height.length; ++i) {
      while (indexStack.peek() != -1 && height[i] < height[indexStack.peek()]) {
        int index = indexStack.pop();
        maxArea = Math.max(maxArea, (i - indexStack.peek() - 1) * height[index]);
      }
      indexStack.push(i);
    }
    while (indexStack.peek() != -1) {
      int index = indexStack.pop();
      maxArea = Math.max(maxArea, 
          (height.length - indexStack.peek() - 1) * height[index]);
    }

    return maxArea;
  }
}
```

