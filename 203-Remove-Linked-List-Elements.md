# 203. Remove Linked List Elements

从链表中去掉所有等于`val`的节点。

建个dummy head，然后从头开始遍历。由于我们要删节点，所以每次要从该节点的前一个节点去访问它，比较之后决定是否删除。删节点很简单，直接让上一个的next跳过这个节点就好。

如果当前节点的值是`val`的话，直接跳过，但是指针不能动，因为我们不知道下一个到底是不是还等于`val`。如果当前节点的值不是`val`的话，指针向后移动继续遍历。

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if(head == null){
            return head;
        }
        ListNode dummyHead = new ListNode(-1, head);
        ListNode p = dummyHead;
        while(p.next != null){
            if(p.next.val == val){
                p.next = p.next.next;
            }else{
                p = p.next;
            }
        }
        return dummyHead.next;
    }
}
```