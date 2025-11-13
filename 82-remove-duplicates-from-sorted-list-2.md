# 82. Remove Duplicates from Sorted List II

参照 [laicode 81](laicode-81-Remove-Adjacent-Repeated-Char-III.md)，也是重复的相邻字符一个不留。这题也是用通用解法，只不过用在链表上。

最后的 slow.next = null 用于砍断后续部分，不可缺少。

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode slow = dummy;
        ListNode fast = head;
        while(fast != null) {
            ListNode begin = fast;
            while(fast != null && fast.val == begin.val) {
                fast = fast.next;
            }
            if(begin.next == fast) {
                slow.next = begin;
                slow = slow.next;
            }
        }
        slow.next = null;
        return dummy.next;
    }
}
```