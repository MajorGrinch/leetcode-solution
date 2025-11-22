# 138. Copy List with Random Pointer

给定一个带有`next`和`random`指针的链表，对它进行deep copy。`random`指针可以指向链表内部任意一个节点，包括自己或者空。

用一个Map来记录新老链表之间节点的对应关系。从原来的表头出发，一直到最后。每轮循环定下当前节点的`next`和`random`元素，然后往后走一直到最后。只要当前节点的`next`不为空，我们首先检查Map里有没有相应的新节点。有的话，直接用，没有的话再创建。再看当前节点的`random`是否为空，不为空我们也接着处理`random`。检查Map里有没有对应的节点在新链表里，有的话直接用，没有的话创建然后用。

这样，最后我们就可以把整个链表的给deep copy一下了。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
  public Node copyRandomList(Node head) {
    if (head == null) {
      return null;
    }
    Map<Node, Node> oldNewMap = new HashMap<>();
    Node newHead = new Node(head.val);
    Node p = head, q = newHead;
    oldNewMap.put(p, q);
    while (p != null) {
      if (p.next != null && !oldNewMap.containsKey(p.next)) {
        oldNewMap.put(p.next, new Node(p.next.val));
      }
      q.next = oldNewMap.get(p.next);
      if (p.random != null && !oldNewMap.containsKey(p.random)) {
        oldNewMap.put(p.random, new Node(p.random.val));
      }
      q.random = oldNewMap.get(p.random);
      p = p.next;
      q = q.next;
    }
    return newHead;
  }
}
```

2025 code:

```java
class Solution {
    public Node copyRandomList(Node head) {
        Node l1 = new Node(0);
        l1.next = head;
        Node dummy = new Node(0);
        Node l2 = dummy;
        Map<Node, Node> oldNewMap = new HashMap<>();
        while (l1.next != null) {
            l2.next = oldNewMap.getOrDefault(l1.next, new Node(l1.next.val));
            oldNewMap.putIfAbsent(l1.next, l2.next);
            if (l1.next.random != null) {
                l2.next.random = oldNewMap.getOrDefault(l1.next.random, new Node(l1.next.random.val));
                oldNewMap.putIfAbsent(l1.next.random, l2.next.random);
            }
            l1 = l1.next;
            l2 = l2.next;
        }
        return dummy.next;
    }
}
```