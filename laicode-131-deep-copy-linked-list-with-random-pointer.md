# Laicode 131. Deep Copy Linked List with Random Pointer

参照[leetcode 138](138-Copy-List-with-Random-Pointer.md)。

```java
public class Solution {
  public RandomListNode copy(RandomListNode head) {
    if(head == null) {
      return head;
    }
    RandomListNode dummy = new RandomListNode(0);
    RandomListNode l1 = head;
    RandomListNode l2 = dummy;
    Map<RandomListNode, RandomListNode> oldNewMap = new HashMap<>();
    while(l1 != null) {
      l2.next = oldNewMap.getOrDefault(l1, new RandomListNode(l1.value));
      oldNewMap.putIfAbsent(l1, l2.next);
      if(l1.random != null) {
        l2.next.random = oldNewMap.getOrDefault(l1.random, new RandomListNode(l1.random.value));
        oldNewMap.putIfAbsent(l1.random, l2.next.random);
      }
      l1 = l1.next;
      l2 = l2.next;
    }
    return dummy.next;
  }
}
```