# QuickSort
### Reference
[Princeton Slides](https://www.cs.princeton.edu/courses/archive/spr10/cos226/lectures/06-23Quicksort-2x2.pdf)

## 1. QuickSort
### Code
#### Java
```java
public class QuickSort {
  public static void sort(Comparable[] elements) {
    sort(elements, 0 , elements.length - 1);
  }

  private static void sort(Comparable[] elements, int start, int end) {
    if (end <= start) {
      return;
    }
    int pivot = partition(elements, start, end);
    sort(elements, start, pivot - 1);
    sort(elements, pivot + 1, end);
  }

  public static int partition(Comparable[] elements, int start, int end) {
    int i = start + 1;
    int j = end;
    while (i <= j) {
      while (i <= end && lessEqual(elements[i], elements[start])) {
        ++i;
      }
      while (j >= i && lessEqual(elements[start], elements[j])) {
        --j;
      }
      if (i < j) {
        swap(elements, i++, j--);
      }
    }
    swap(elements, start, j);
    return j;
  }

  private static boolean lessEqual(Comparable element1, Comparable element2) {
    return element1.compareTo(element2) <= 0;
  }

  private static void swap(Comparable[] elements, int i, int j) {
    Comparable tmp = elements[i];
    elements[i] = elements[j];
    elements[j] = tmp;
  }
}
```

## 2. 3-way QuickSort 
3-way QuickSort improve QuickSort when there are lots of duplicate values in the array. 

### Code
#### Java
```java
public class QuickSort3Way {
  public static void sort(Comparable[] elements) {
    sort(elements, 0, elements.length - 1);
  }

  private static void sort(Comparable[] elements, int start, int end) {
    if (end <= start) {
      return;
    }
    int lt = start;
    int gt = end;
    int i = start + 1;
    Comparable v = elements[start];
    while (i <= gt) {
      int cmp = elements[i].compareTo(v);
      if (cmp < 0) {
        swap(elements, i++, lt++);
      } else if (cmp > 0) {
        swap(elements, i, gt--);
      } else {
        i++;
      }
    }
    sort(elements, start, lt - 1);
    sort(elements, gt + 1, end);
  }

  private static void swap(Comparable[] elements, int i, int j) {
    Comparable tmp = elements[i];
    elements[i] = elements[j];
    elements[j] = tmp;
  }
}
```

