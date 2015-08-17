# QuickSelect
## Reference
[Princeton Slides](https://www.cs.princeton.edu/courses/archive/spr10/cos226/lectures/06-23Quicksort-2x2.pdf)

## Note
The algorithm can choose the kth smallest/largest element from an unorder array in O(N) time. 

### Code
#### Java
```java
public class QuickSelect {
  public static Comparable select(Comparable[] elements, int k) {
    int start = 0;
    int end = elements.length - 1;
    while (start < end) {
      int pivot = partition(elements, start, end);
      if (pivot < k) {
        start = pivot + 1;
      } else if (pivot > k) {
        end = pivot - 1;
      } else {
        break;
      }
    }
    return elements[k];
  }

  private static int partition(Comparable[] elements, int start, int end) {
    int i = start + 1;
    int j = end;
    while (i <= j) {
      while (i <= j && less(elements[i], elements[start])) {
        i++;
      }
      while (i <= j && lessEqual(elements[start], elements[j])) {
        j--;
      }
      if (i < j) {
        swap(elements, i, j);
      }
    }
    swap(elements, start, j);
    return j;
  }

  private static boolean lessEqual(Comparable element1, Comparable element2) {
    return element1.compareTo(element2) <= 0;
  }

  private static boolean less(Comparable element1, Comparable element2) {
    return element1.compareTo(element2) < 0;
  }

  private static void swap(Comparable[] elements, int i, int j) {
    Comparable tmp = elements[i];
    elements[i] = elements[j];
    elements[j] = tmp;
  }
```
