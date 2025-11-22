# Laicode 40. Merge Two Sorted Linked Lists

参照[leetcode 21](21-Merge-Two-Sorted-Lists.md)。

```java
public class Solution {
  public ListNode merge(ListNode one, ListNode two) {
    ListNode dummy = new ListNode(0);
    ListNode l = dummy;
    while(one != null && two != null) {
      if(one.value < two.value) {
        l.next = one;
        one = one.next;
      } else {
        l.next = two;
        two = two.next;
      }
      l = l.next;
    }
    if(one != null) {
      l.next = one;
    }
    if(two != null) {
      l.next = two;
    }
    return dummy.next;
  }
}
```