# 23. Merge k Sorted Lists

给定k个有序链表，把他们合并为一个有序链表。

不是很懂，这也能算hard题？是我所在的层数太低了么？难道我在第一层而这题在第五层？

新建一个dummy表头用来存放答案。用小顶堆存放所有链表的表头然后开始做以下两件事：
+ 从小顶堆里取出当前最小的节点加入答案。
+ 如果当前这个节点的next不为空的话，那就把它放入小顶堆。

重复上述两件事情直到堆为空，最后我们就得到了一个有序的链表。

Time complexity: O(N * k * logk) where k is the number of lists and N is the total number of nodes of all k lists.

Space complexity: O(k). Min heap will have k elements most of the time.

```java
class Solution {
  public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
      return null;
    }
    int k = lists.length;
    if (k == 1) return lists[0];

    PriorityQueue<ListNode> minHeap =
        new PriorityQueue<>(
            new Comparator<ListNode>() {
              @Override
              public int compare(ListNode p1, ListNode p2) {
                return Integer.compare(p1.val, p2.val);
              }
            });
    for (int i = 0; i < k; i++) {
      if (lists[i] != null) minHeap.offer(lists[i]);
    }
    ListNode dummyHead = new ListNode(0);
    ListNode p = dummyHead;
    while (!minHeap.isEmpty()) {
      ListNode curr = minHeap.poll();
      p.next = curr;
      p = p.next;
      if (curr.next != null) minHeap.offer(curr.next);
    }
    return dummyHead.next;
  }
}
```

2025 code:

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> minHeap = new PriorityQueue<>(new Comparator<ListNode>() {
            @Override
            public int compare(ListNode l1, ListNode l2) {
                return Integer.compare(l1.val, l2.val);
            }
        });
        for (ListNode node : lists) {
            if (node != null) {
                minHeap.offer(node);
            }
        }
        ListNode dummy = new ListNode(0);
        ListNode l = dummy;
        while (!minHeap.isEmpty()) {
            ListNode curr = minHeap.poll();
            l.next = curr;
            l = l.next;
            curr = curr.next;
            if (curr != null) {
                minHeap.offer(curr);
            }
        }
        return dummy.next;
    }
}
```