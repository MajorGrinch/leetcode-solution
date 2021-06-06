# Laicode 30. Quick Sort Linked List

对链表进行快排。

我们思考一下快排的过程，无非就是先`partition`然后对前后部分递归的进行相同的操作直至不可分，然后整个数组就有序了。那么这里对链表进行快排也是一个道理。

`partition(ListNode[] boundary)`方法实现一个给定链表进行partition操作，并把新的`head`和`tail`放回`boundary`。pivot选取的是给定链表的尾部元素。快排的partition是需要返回pivot的位置的，我们这里用`smallTail`来返回较小的那部分的尾部。那么显然，`smallTail.next == pivot`。但是，如果给定链表里剩余的元素都比`pivot`大，那么`smallTail`就会为`null`。

`quickSort(ListNode[] boundary)`方法实现对给定链表进行快排操作，并把新的`head`和`tail`放回`boundary`。`partition`完之后，需要对`pivot`左右两边各自进行递归调用。正如我之前所说的，`pivot`前后可能为空，所以我们需要检查一下。`smallTail == null`说明前面部分为空，`newTail == pivot == tail`，说明后面为空。前面不为空的话，就对前面进行递归调用。递归调用之前，切记切记让`smallTail.next = null`使得前面成为一个独立的链表。调用返回之后，前面有序的头结点就是我当前链表新的头结点，前面有序的尾结点接上`pivot`。后面不为空的话，再对后面部分进行递归调用。后面有序的尾结点就是我当前链表新的尾结点，把`pivot`接上后面有序的头结点。

这样，最后整体链表就有序了，并存在数组里。最后我们返回数组首个元素，也就是新链表的头结点。

```java
public class Solution {
  public ListNode quickSort(ListNode head) {
    if (head == null || head.next == null) {
      return head;
    }
    ListNode tail = head;
    while (tail.next != null) tail = tail.next;

    ListNode[] boundary = {head, null, tail};
    quickSort(boundary);
    return boundary[0];
  }

  /**
   * Given a linked list, with head and tail as boundary[0] and boundary[2], perform quick sort on
   * it. Update the head and tail in the boundary.
   */
  private void quickSort(ListNode[] boundary) {
    ListNode head = boundary[0], tail = boundary[2];
    if (head == null || head == tail) {
      return;
    }
    partition(boundary);
    ListNode newHead = boundary[0], smallTail = boundary[1], newTail = boundary[2];
    ListNode pivot = tail;
    if (smallTail != null) {
      // there are elements before pivot
      smallTail.next = null;
      boundary[0] = newHead;
      boundary[2] = smallTail;
      quickSort(boundary);
      newHead = boundary[0];
      boundary[2].next = pivot;
    }
    if (newTail != pivot) {
      // there are elements after pivot
      boundary[0] = pivot.next;
      boundary[2] = newTail;
      quickSort(boundary);
      newTail = boundary[2];
      pivot.next = boundary[0];
    }
    boundary[0] = newHead;
    boundary[2] = newTail;
  }

  /**
   * Given a linked list, with head and tail as boundary[0] and boundary[2], perform partition on
   * it. Update the head and tail in the boundary. Put the tail of smaller part in boundary[1] which
   * could be null if all elements are larger than pivot.
   */
  private void partition(ListNode[] boundary) {
    ListNode head = boundary[0], tail = boundary[2];
    ListNode pivot = tail;
    ListNode curr = head;
    ListNode newHead = null, smallTail = null, newTail = null;
    while (curr != tail) {
      if (curr.value < pivot.value) {
        if (newHead == null) newHead = curr;
        smallTail = curr;
        curr = curr.next;
      } else {
        if (newTail == null) newTail = curr;
        if (smallTail != null) smallTail.next = smallTail.next.next;
        ListNode currNext = curr.next;
        curr.next = pivot.next;
        pivot.next = curr;
        curr = currNext;
      }
    }
    if (newHead == null) newHead = pivot;
    if (newTail == null) newTail = pivot;
    boundary[0] = newHead;
    boundary[1] = smallTail;
    boundary[2] = newTail;
  }
}
```