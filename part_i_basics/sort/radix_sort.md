# Radix Sort
### Reference
[Radix Sort Priceton Slides](https://www.cs.princeton.edu/~rs/AlgsDS07/18RadixSort.pdf)

### Code
#### Java
```java
public class RadixSort {
  private static int[] radixSort(int[] nums) {
    for (int exp = 1; exp > 0; exp *= 10) {
      nums = countSort(nums, exp);
    }

    return nums;
  }

  private static int[] countSort(int[] nums, int exp) {
    int[] result = new int[nums.length];
    int[] count = new int[10];

    // count occurence of the digit
    for (int i = 0; i < nums.length; ++i) {
      ++count[(nums[i] / exp) % 10];
    }

    // compute actual position of that digit in count
    for (int i = 1; i < 10; ++i) {
      count[i] += count[i - 1];
    }

    // sort
    for (int i = nums.length - 1; i >= 0; --i) {
      result[--count[(nums[i] / exp) % 10]] = nums[i];
    }

    return result;
  }
}
```
