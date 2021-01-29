# 141. Linked List Cycle

确定链表内部是否有环，依然是快慢指针来解决。

依然是快慢指针一起从头结点出发，慢指针走一步，快指针走两步。理论上来说，如果链表无环的话，快慢指针永远不可能相遇，最后一定是快指针走到链表尽头从而结束。

但是如果链表有环的话，快指针就会首先进入环里，慢指针随后再进入。快指针每回合都会比慢指针多走一步，所以最后一定会再一次赶上慢指针，就像运动会上别人已经比你多跑了整整一圈一样。

Time complexity: O(n)

Space complexity: O(1)

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null){
            return false;
        }
        ListNode slow = head, fast = head;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(fast == slow){
                return true;
            }
        }
        return false;
    }
}
```