# 35. Reverse Linked List In Pairs

## Recursive Approach

成对的反转链表，比如`1->2->3->4->5->#`变成`2->1->4->3->5->#`。

那其实就是一对一对的反转，反转后的新头传回去给上一对反转时候用。

Time complexity: O(n)

Space complexity: O(n), because n/2 level call stacks.

```java
public class Solution {
  public ListNode reverseInPairs(ListNode head) {
    if(head == null || head.next == null){
      return head;
    }
    ListNode next = reverseInPairs(head.next.next);
    ListNode newHead = head.next;
    head.next.next = head;
    head.next = next;
    return newHead;
  }
}
```