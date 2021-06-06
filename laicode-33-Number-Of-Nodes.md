# 33. Number Of Nodes

给定一个链表求出它节点个数，没啥好说的。

```java
public class Solution {
  public int numberOfNodes(ListNode head) {
    ListNode p = head;
    int count = 0;
    while (p != null) {
      p = p.next;
      count++;
    }
    return count;
  }
}
```

## Show off

此为炫技，面试时请勿模仿。

```java
public class Solution {
  public int numberOfNodes(ListNode head) {
    ListNode p = head;
    int count = 0;
    while (p != null && ++count > 0) p = p.next;
    return count;
  }
}
```