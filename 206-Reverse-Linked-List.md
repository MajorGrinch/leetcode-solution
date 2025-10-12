# 206 Reverse Linked List

顾名思义，反转链表。不过虽然比较简单，但是有两种不同的实现方法。递归实现和迭代实现。

## Iterative Method

这里我们定义两个指针，`curr`表示当前正在处理的节点，`prev`表示上一个被处理的节点，然后遍历这个链表。

刚开始他们都是null，因为还没有开始遍历。开始遍历到head为null为止。每次先让curr指向head指向的节点，紧接着就把head往后移，因为我们不想影响遍历的过程。

为了反转链表，那么`curr`就得指向上一个被处理的节点，即`prev`。进入下一轮循环前，让 `prev`指向`curr`同一个节点就好。下一轮curr会自动指向下一个节点的，直到head为null。head为null之后，那么这时候`curr`指向的也就是原来链表的最后一个节点，也就是反转过后的第一个节点，直接返回就好。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode curr = null, prev = null;
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

2025 new approach:

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null)
            return head;

        ListNode newHead = new ListNode(-1);
        ListNode p = head;
        while (p != null) {
            ListNode next = p.next;
            p.next = newHead.next;
            newHead.next = p;
            p = next;
        }
        return newHead.next;
    }
}
```

## Recursive Method


递归的方法来反转链表，先一直递归到链表的结尾即 `head.next == null` 此时这个head就是我们反转之后的新的链表头，直接将它返回给上一层调用。

当前层拿到下一层调用返回的 `newHead` 之后肯定是也要返回这个`newHead`的，不然第一层调用也拿不到 `newHead` 作为答案返回。不过当前层除了要返回 `newHead`，还需要对当前的`head`进行反转处理。`head.next`在原来链表里面是`head`的下一个节点，这里我们就让`head.next.next = head`，语义上就是把`head`和`head.next`翻转了一下。不过还要注意把 `head.next`置为null，让它指向空，防止链表出现循环。

这样最开始的调用就可以拿到 `newHead` 并且整个链表也被翻转了。

Corner case: 输入的链表本身就是空。

Time complexity: O(N)

Space complexity: O(N), because of N levels of call stack.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```