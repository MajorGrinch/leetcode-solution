# Laicode 39. Inser In Sorted Linked List

给定一个有序链表，把`value`插入到正确的位置。

那就一直找到第一个大于等于`value`的节点的前一个节点。在head之前接一个dummy head作为起始点，插入之后直接返回dummy的下一个点就是head.

Time complexity: O(N)

Space complexity: O(1)

```java
public class Solution {
    public ListNode insert(ListNode head, int value) {
      ListNode node = new ListNode(value);
      if(head == null){
        return node;
      }
      ListNode dummyHead = new ListNode(0);
      dummyHead.next = head;
      ListNode p = dummyHead;
      while(p.next != null && p.next.value < value){
        p = p.next;
      }
      node.next = p.next;
      p.next = node;
      return dummyHead.next;
    }
}
```