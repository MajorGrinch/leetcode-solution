# 146. LRU Cache

实现一个LRU cache。

LRU, least recent used， is a cache replacement policy. 当缓存空间不够的时候，挑出最久没有被访问的缓存项丢弃。

这里我们需要一个双端队列来存放缓存数据，还需要一个Map来快速获取缓存项，`cnt`来记录目前有多少缓存项，以及`capacity`记录缓存的大小。`get`的时候，先检查对应的缓存是否存在。不存在的话，直接按照题意返回`-1`。存在的话，在返回缓存项的值之前，先把缓存项放到队列的头部，表示这个最近刚被访问过过。

`put`的时候，先通过Map来判断给定的`key`是否存在。存在的话，就直接修改对应缓存项的值，并且把缓存项放到队列头部表示刚被访问过。不存在的话，就新建节点然后放入队列头部和Map。如果放入之后，缓存项的数量超过了上限，那么就要从队列尾部弹出一个缓存项丢弃掉，并且从Map里也删去这个缓存项的键值对。

这里双端队列的实现，我们也是手动实现的，因为把队列中的某个元素放到队列头部这个操作需要我们先删掉它，然后把它重新放入头部。java自带的ArrayDeque和LinkedList都会先遍历一遍队列找到对应的元素，会有性能上的损失。我们这里自己实现一个`remove(DLinkedListNode node)`来快速删除。


```java
class LRUCache {

  private Map<Integer, DLinkedListNode> nodeMap;
  private final int capacity;
  private int cnt;
  private DLinkedList cache;

  public LRUCache(int capacity) {
    this.nodeMap = new HashMap<>();
    this.capacity = capacity;
    this.cnt = 0;
    this.cache = new DLinkedList();
  }

  public int get(int key) {
    DLinkedListNode res = nodeMap.get(key);
    if (res == null) {
      return -1;
    }
    cache.moveToFirst(res);
    return res.val;
  }

  public void put(int key, int value) {
    DLinkedListNode node = nodeMap.get(key);
    if (node != null) {
      node.val = value;
      cache.moveToFirst(node);
    } else {
      node = new DLinkedListNode(key, value);
      cache.offerFirst(node);
      nodeMap.put(key, node);
      if (++cnt > capacity) {
        DLinkedListNode discard = cache.pollLast();
        nodeMap.remove(discard.key);
        cnt--;
      }
    }
  }
}

class DLinkedList {
  DLinkedListNode head;
  DLinkedListNode rear;

  DLinkedList() {
    head = new DLinkedListNode(-1, -1);
    rear = new DLinkedListNode(-1, -1);
    head.next = rear;
    rear.prev = head;
  }

  public void offerFirst(DLinkedListNode node) {
    node.next = head.next;
    node.next.prev = node;
    node.prev = head;
    head.next = node;
  }

  private DLinkedListNode removeNode(DLinkedListNode node) {
    node.prev.next = node.next;
    node.next.prev = node.prev;
    node.next = null;
    node.prev = null;
    return node;
  }

  public DLinkedListNode pollLast() {
    return removeNode(rear.prev);
  }

  public void moveToFirst(DLinkedListNode node) {
    removeNode(node);
    offerFirst(node);
  }
}

class DLinkedListNode {
  int key;
  int val;

  DLinkedListNode prev;
  DLinkedListNode next;

  DLinkedListNode(int key, int val) {
    this.key = key;
    this.val = val;
  }
}
```