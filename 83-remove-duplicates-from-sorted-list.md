# 83. Remove Duplicates from Sorted List

参考[leetcode 26](26-Remove-Duplicates-From-Sorted-Array.md)的通用解法，对链表也同样适用，只不过需要一些细微的改动。

最后的 slow.next = null 用于砍断后续部分，不可缺少。

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode slow = dummy;
        ListNode fast = dummy.next;
        while (fast != null) {
            ListNode begin = fast;
            while (fast != null && fast.val == begin.val) {
                fast = fast.next;
            }
            slow.next = begin;
            slow = slow.next;
        }
        slow.next = null;
        return dummy.next;
    }
}
```