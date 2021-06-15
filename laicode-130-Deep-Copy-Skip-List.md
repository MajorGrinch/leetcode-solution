# Laicode 130. Deep Copy Skip List

拷贝一个skip list，forward指针只会指后面的节点。和[138. Copy List with Random Pointer](138-Copy-List-with-Random-Pointer.md)类似，但是我们这里有个小改动。在拷贝`next`的时候，可以不用放入Map，当前点的`next`以后不可能用得着的，因为`forward`指针只会指向后面的点不像`random pointer`会指向之前的点。

Time complexity: O(N)

Space complexity: O(N)

```java
public class Solution {
  public SkipListNode copy(SkipListNode head) {
    if (head == null) {
      return head;
    }
    Map<SkipListNode, SkipListNode> oldNewMap = new HashMap<>();
    SkipListNode newHead = new SkipListNode(head.value);
    oldNewMap.put(head, newHead);
    SkipListNode curr = newHead;
    while (head != null) {
      if(head.forward != null){
        if(oldNewMap.containsKey(head.forward)){
          curr.forward = oldNewMap.get(head.forward);
        }else{
          curr.forward = new SkipListNode(head.forward.value);
          oldNewMap.put(head.forward, curr.forward);
        }
      }
      if(head.next != null){
        if(oldNewMap.containsKey(head.next)){
          curr.next = oldNewMap.get(head.next);
        }else{
          curr.next = new SkipListNode(head.next.value);
        }
      }
      head = head.next;
      curr = curr.next;
    }

    return newHead;
  }
}
```