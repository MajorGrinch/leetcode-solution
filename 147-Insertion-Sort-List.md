# 147. Insertion Sort List

对一个链表实施Insertion Sort。

其实Insertion Sort原理就是`j`从第二个元素到最后，[0, j - 1]有序，每次从`[j, len)`里拿出`j`找到它在`[0, j - 1]`里的位置插入进去，使得`[0, j]`有序。

对应到这个链表反而好搞。我们新起一个节点，遍历原来的链表里的每个节点，在新链表里面找到对应位置并插入。

Time complexity: O(n^2)

Space complexity: O(1)

```java
class Solution {
  public ListNode insertionSortList(ListNode head) {
    if (head == null || head.next == null) {
      return head;
    }
    ListNode dummyHead = new ListNode(-1);
    ListNode curr = head;
    while (curr != null) {
      ListNode insertPrev = dummyHead;
      while (insertPrev.next != null && insertPrev.next.val < curr.val) {
        insertPrev = insertPrev.next;
      }

      ListNode currNext = curr.next;
      curr.next = insertPrev.next;
      insertPrev.next = curr;
      curr = currNext;
    }
    return dummyHead.next;
  }
}
```