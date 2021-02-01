# 86. Partition List

新开两个链表头结点，一个lt (less than) 接所有比x小的节点，一个ge (greater than or equal to) 接所有大于等于x的节点，然后把他们拼起来就好。

不过需要注意的是 ge 链表最后一个节点的next需要置为`null`防止出现环。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode ltHead = new ListNode(-1);
        ListNode geHead = new ListNode(-1);
        ListNode p = head;
        ListNode ltTail = ltHead, geTail = geHead;
        while(p != null){
            if(p.val < x){
                ltTail.next = p;
                ltTail = ltTail.next;
            }else{
                geTail.next = p;
                geTail = geTail.next;
            }
            p = p.next;
        }
        ltTail.next = geHead.next;
        geTail.next = null;
        return ltHead.next;
    }
}
```