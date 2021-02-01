# 143. Reorder List

给定一个链表，把它按照题目规则重新排列。

本质上其实就是把原来的链表分成两半，把第二半翻转一下，然后1对1合并。

Time complexity: O(N)

Space complexity: O(1), or O(N) if you use recursive way to reverse the list.

```java
class Solution {
    public void reorderList(ListNode head) {
        if(head == null || head.next == null){
            return;
        }
        ListNode bfMid = getMiddleElseBefore(head);
        ListNode head1 = head;
        ListNode head2 = bfMid.next;
        bfMid.next = null;  // avoid cycle
        head2 = reverseList(head2);
        while(head1 != null && head2 != null){
            ListNode ln1 = head1, ln2 = head2;
            head1 = head1.next;
            head2 = head2.next;
            ln1.next = ln2;
            ln2.next = head1;
        }
    }

    /**
     * Given a no-empty list starts with head. If the size is even, get the tail of first half.
     * Otherwise, get the middle.
     */
    private ListNode getMiddleElseBefore(ListNode head){
        ListNode slow = head, fast = head.next;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    /**
     * Given a no-empty list starts with head, reverse it.
     */
    private ListNode reverseList(ListNode head){
        ListNode prev = null, curr = null;
        while(head != null){
            curr = head;
            head = head.next;
            curr.next = prev;
            prev = curr;
        }
        return curr;
    }
}
```