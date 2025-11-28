# Laicode 205. Implement LRU Cache

参照[leetcode 146](146-LRU-Cache.md)。

```java
public class Solution<K, V> {
  private Map<K, Node> cache;
  private DLinkedList usage;
  private int capacity;

  public Solution(int limit) {
    this.capacity = limit;
    cache = new HashMap<>();
    usage = new DLinkedList();  
  }
  
  public void set(K key, V value) {
    Node node = this.cache.get(key);
    if(node != null) {
      node.value = value;
      this.usage.moveToFirst(node);
    } else {
      node = new Node(key, value);
      this.cache.put(key, node);
      this.usage.offerFirst(node);
      if(this.cache.size() > capacity) {
        this.cache.remove(this.usage.pollLast().key);
      }
    }
  }
  
  public V get(K key) {
    Node node = this.cache.get(key);
    if(node == null) {
      return null;
    }
    this.usage.moveToFirst(node);
    return node.value;
  }

  class DLinkedList {
    Node head;
    Node tail;

    DLinkedList() {
      head = new Node(null, null);
      tail = new Node(null, null);
      head.next = tail;
      tail.prev = head;
    }

    public void offerFirst(Node node) {
      node.next = head.next;
      node.next.prev = node;
      head.next = node;
      node.prev = head;
    }

    private Node removeNode(Node node) {
      node.prev.next = node.next;
      node.next.prev = node.prev;
      node.next = null;
      node.prev = null;
      return node;
    }

    public void moveToFirst(Node node) {
      this.removeNode(node);
      this.offerFirst(node);
    }

    public Node pollLast() {
      return this.removeNode(this.tail.prev);
    }
  }

  class Node {
    K key;
    V value;
    Node prev;
    Node next;

    Node(K key, V value) {
      this.key = key;
      this.value = value;
    }
  }
}
```