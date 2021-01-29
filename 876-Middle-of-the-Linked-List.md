# 876. Middle of the Linked List

找链表的中间节点，如果是偶数个那就中间两个的后一个。

这题可以用快慢指针来找中间点，慢指针一次走一步，快指针一次走两步。当快指针走到头了，慢指针应该就走到了中间点。不过如何确定慢指针就是落到了我们要的那个中间的节点，需要认真论证一下。我们不妨分奇偶数来考虑，即链表有`2n`和`2n+1`个元素这两种情况。

当链表有`2n`个节点时，我们假定起点编号是1，那么我要找的节点是`n + 1`，`(1, n + 1]`有n个点，`(1, 2n]`则有`2n - 1`个点。那么慢指针走到`n + 1`需要走n次，快指针走到尽头也需要`n`次。

当链表有`2n + 1`个节点是，那么我们要找的节点依然是`n + 1`，`(1, n + 1]`有n个点，`(1, 2n+1]`则有`2n`个点。那么慢指针走到`n + 1`需要走n次，快指针走到尽头依然需要`n`次。

综上，快慢指针同时从首个节点出发，按照各自步长行走。当快指针走到尽头时，慢指针就正好停在了题目所规定的中间点。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode slow = head, fast = head;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```
