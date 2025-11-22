# Laicode 134. Merge K Sorted Lists

参照[leetcode 23](23-Merge-K-Sorted-Lists.md)。

```java
public class Solution {
  public ListNode merge(List<ListNode> listOfLists) {
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>(new Comparator<ListNode>() {
      @Override
      public int compare(ListNode l1, ListNode l2) {
        return Integer.compare(l1.value, l2.value);
      }
    });
    for(ListNode l: listOfLists) {
      minHeap.offer(l);
    }
    ListNode dummy = new ListNode(0);
    ListNode l = dummy;
    while(!minHeap.isEmpty()) {
      ListNode curr = minHeap.poll();
      l.next = curr;
      l = l.next;
      curr = curr.next;
      if(curr != null) {
        minHeap.offer(curr);
      }
    }
    return dummy.next;
  }
}
```