# Laicode 28. Selection Sort Linked List

对链表进行 Selection Sort。

Selection sort的核心思想是，`i`从`[0, len - 2]`，每次从`[i, len - 1]`里面拿出最小的放到`i`，这样最后就整体有序了。这个思想用到链表上依然还是双指针，`i`从头结点遍历到倒数第二个节点，每次从`i`开始的剩余部分找到最小的然后放到`i`这个位置。

但是这题因为是链表，链表把元素放到前面去需要获得这个节点的前一个节点，所以我们用`iNodePrev`和`jNodePrev`来当做`i`和`j`的前置节点，放一个`dummyHead`在整个链表之前。`iNodePrev`从`dummyHead`遍历到最后，`jNodePrev`从`iNodePrev`遍历到最后，并从剩余部分里找到最小节点`localMin`的前置节点赋给`localMinPrev`。利用`localMinPrev`把`localMin`给删掉，并且把`localMin`给放到`iNodePrev`后面，然后`iNodePrev`向后走一步。

最后整个链表就有序了。

Time complexity: O(n^2)

Space complexity: O(1)

```java
public class Solution {
  public ListNode selectionSort(ListNode head) {
    if (head == null || head.next == null) {
      return head;
    }
    ListNode dummyHead = new ListNode(-1);
    dummyHead.next = head;
    ListNode iNodePrev = dummyHead;
    while (iNodePrev.next != null) {
      ListNode jNodePrev = iNodePrev;
      ListNode localMinPrev = iNodePrev;
      while (jNodePrev.next != null) {
        if (localMinPrev.next.value > jNodePrev.next.value) {
          localMinPrev = jNodePrev;
        }
        jNodePrev = jNodePrev.next;
      }
      if (localMinPrev == iNodePrev) {
        iNodePrev = iNodePrev.next;
        continue;
      }
      ListNode localMin = localMinPrev.next;
      localMinPrev.next = localMin.next;
      localMin.next = iNodePrev.next;
      iNodePrev.next = localMin;
      iNodePrev = iNodePrev.next;
    }
    return dummyHead.next;
  }
}
```