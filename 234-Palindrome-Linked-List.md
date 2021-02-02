# 234. Palindrome Linked List

检测一个链表是否为回文。

常规的方法是拷贝一份这个链表并且反转一下，和原链表对比就行。这个很简单，不多说明。这里我们关注一下`follow up`，要求O(1)的空间复杂度。

常数级的空间复杂度就意味着上个方法不行了，所以我们这里另起一个方法。找到链表的中间点，或者前一半的尾部。把前后两半变成两个独立的链表，再把后一半给反转一下。

由于我找的是前一半的尾部或者中间点，所以左边那半的链表长度一定只可能等于右边或者比右边多1。所以我们对两个链表进行遍历，以右边的结束为准，遇到不同的说明不是回文。如果右边遍历结束了，没有遇到不同，说明这个是回文。

另外，这里其实找中间点的时候，如果长度是偶数，找前一半的尾部和找后一半的头部都一样。只不过，找前一半的尾部之后通过`mid.next = null`，我可以把这两半变成真正独立的链表，方便后续再变回来。找后一半头部，就不行，左边的尾部会继续指向反转之后右边的尾结点。但是就算法正确性来说，并不影响。因为右边遍历结束时，左边是不可能遍历到右边的尾结点的。

Time complexity: O(N)

Space complexity: O(1)，注意不要用recursion来反转链表，否则空间复杂度就不是O(1)了。

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null){
            return true;
        }
        ListNode mid = getMiddleElseBefore(head);
        ListNode left = head, right = mid.next;
        mid.next = null;
        right = reverse(right);
        while(right != null){
            if(left.val != right.val){
                return false;
            }
            left = left.next;
            right = right.next;
        }
        return true;
    }

    /**
     * Given a no-empty linked list, reverse it.
     */
    private ListNode reverse(ListNode head){
        if(head.next == null){
            return head;
        }
        ListNode prev = null, curr = null;
        while(head != null){
            curr = head;
            head = head.next;
            curr.next = prev;
            prev = curr;
        }
        return curr;
    }

    /**
     * Given an no-empty linked list, return its middle node if the size is odd.
     * Else return the tail of the left half.
     */
    private ListNode getMiddleElseBefore(ListNode head){
        if(head.next == null){
            return head;
        }
        ListNode slow = head, fast = head.next;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```