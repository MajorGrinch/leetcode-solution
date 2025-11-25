# Laicode 653. Reverse Linked List (recursive)

参照[leetcode 206](206-Reverse-Linked-List.md)。

```java
public class Solution {
  public ListNode reverse(ListNode head) {
    if(head == null || head.next == null) {
      return head;
    }
    ListNode headNew = reverse(head.next);
    head.next.next = head;
    head.next = null;
    return headNew;
  }
}
```