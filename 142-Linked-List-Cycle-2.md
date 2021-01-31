# 142. Linked List Cycle II

链表有环的话，返回环的起点，否则返回null。

和[141](141-Linked-List-Cycle.md)一样这题利用快慢指针思想，先确定是否有环。当fast超过一圈赶上slow之后，此时fast走过的路程正好是slow的两倍。

那么我们可以推理出，环的起点到链表起点的距离正好等于此刻slow位置向前走到环起点的距离。

另起一个点从链表起点开始，然后slow继续走，两个点最后一定会在环的起点相遇。

Time complexity: O(N)

Space complexity: O(1)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null){
            return null;
        }
        ListNode slow = head, fast = head;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast){
                break;
            }
        }
        if(slow != fast){
            return null;
        }
        ListNode start = head;
        while(start != slow){
            start = start.next;
            slow = slow.next;
        }
        return start;
    }
}
```