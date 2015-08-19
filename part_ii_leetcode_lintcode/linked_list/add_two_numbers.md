# Add Two Numbers
> You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.
>
> Example
>
> Given 7->1->6 + 5->9->2. That is, 617 + 295.
>
> Return 2->1->9. That is 912.
>
> Given 3->1->5 and 5->9->2, return 8->0->8.

### Analysis
Simply traverse the linkedList

### Complexity
Time: O(N)

Space: O(1)

### Code
#### Java
```java
public class Solution {
  private static final int BASE = 10;

  public ListNode addLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(-1);
    ListNode tail = dummy;
    int carry = 0;
    while (l1 != null || l2 != null || carry != 0) {
      int num1 = 0;
      if (l1 != null) {
        num1 = l1.val;
        l1 = l1.next;
      }
      int num2 = 0;
      if (l2 != null) {
        num2 = l2.val;
        l2 = l2.next;
      }
      carry += num1 + num2;
      tail = tail.next = new ListNode(carry % BASE);
      carry /= BASE;
    }
    return dummy.next;
  }
}
```
