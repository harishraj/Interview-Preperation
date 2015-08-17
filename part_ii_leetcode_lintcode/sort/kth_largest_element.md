# Kth Largest Element
> Find K-th largest element in an array.
> 
> Have you met this question in a real interview? Yes
>
> Example
>
> In array [9,3,2,4,8], the 3rd largest element is 4.
>
> In array [1,2,3,4,5], the 1st largest element is 5, 2nd largest element is 4, 3rd largest element is 3 and etc.

## Method 1: Heap
### Analysis
Simply put all numbers into a max heap and remove from it until find the kth number.

### Complexity
Time: O(NlgN)

Space: O(N)

### Code
#### Java
```java
class Solution {
  public int kthLargestElement(int k, ArrayList<Integer> numbers) {
    Queue<Integer> pq = new PriorityQueue<>(numbers.size(), Collections.reverseOrder());
    for (Integer num : numbers) {
      pq.offer(num);
    }
    int num = 0;
    for (int i = 0; i < k; i++) {
      num = pq.poll();
    }
    return num;
  }
}
```

## Method 2: Quick Select
### Analysis
Use quick select algorithm.

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
class Solution {
  public int kthLargestElement(int k, int[] numbers) {
    int start = 0;
    int end = numbers.length - 1;
    k -= 1
    while (start <= end) {
      int pivot = partition(numbers, start, end);
      if (pivot < k) {
        start = pivot + 1;
      } else if (pivot > k) {
        end = pivot - 1;
      } else {
        break;
      }
    }
    return numbers[k];
  }

  private int partition(int[] numbers, int start, int end) {
    int i = start + 1;
    int j = end;
    while (i <= j) {
      while (i <= j && numbers[i] > numbers[start]) {
        i++;
      }
      while (i <= j && numbers[j] <= numbers[start]) {
        j--;
      }
      if (i < j) {
        swap(numbers, i, j);
      }
    }
    swap(numbers, start, j);
    return j;
  }

  private void swap(int[] numbers, int index1, int index2) {
    int tmp = numbers[index1];
    numbers[index1] = numbers[index2];
    numbers[index2] = tmp;
  }
}
```
