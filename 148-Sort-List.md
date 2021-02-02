# 148. Sort List

对链表进行排序，这里用归并排序。归并排序的思想不变，依然还是先拆再合并。

实现的时候要注意链表成环就好。

Time complexity: O(NlogN)

Space complexity: O(1), no extra space introduced.

```java
class Solution {
    public ListNode sortList(ListNode head) {
        return mergeSort(head);
    }

    private ListNode mergeSort(ListNode head){
        if(head == null || head.next == null){
            return head;
        }
        ListNode mid = getMiddleElseBefore(head);
        ListNode l1 = head, l2 = mid.next;
        mid.next = null;
        l1 = mergeSort(l1);
        l2 = mergeSort(l2);
        return merge(l1, l2);
    }

    private ListNode getMiddleElseBefore(ListNode head){
        ListNode slow = head, fast = head.next;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    private ListNode merge(ListNode l1, ListNode l2){
        ListNode newHead;
        if(l1.val <= l2.val){
            newHead = l1;
            l1 = l1.next;
        }else{
            newHead = l2;
            l2 = l2.next;
        }
        ListNode p = newHead;
        while(l1 != null && l2 != null){
            if(l1.val <= l2.val){
                p.next = l1;
                l1 = l1.next;
            }else{
                p.next = l2;
                l2 = l2.next;
            }
            p = p.next;
        }
        if(l1 != null){
            p.next = l1;
        }
        if(l2 != null){
            p.next = l2;
        }
        return newHead;
    }
}
```