# 146. LRU Cache

实现一个LRU cache。

LRU, least recent used， is a cache replacement policy. 当缓存空间不够的时候，挑出最久没有被访问的缓存项丢弃。访问包括 put 和 get 两个操作。

这里我们需要一个双端队列来存放缓存数据以用来记录recent usage，还需要一个Map来快速获取缓存项，以及`capacity`记录缓存的大小。`get`的时候，先检查对应的缓存是否存在。不存在的话，直接按照题意返回`-1`。存在的话，在返回缓存项的值之前，先把缓存项放到队列的头部，表示这个最近刚被访问过过。

`put`的时候，先通过Map来判断给定的`key`是否存在。存在的话，就直接修改对应缓存项的值，并且把缓存项放到队列头部表示刚被访问过。不存在的话，就新建节点然后放入队列头部和Map。如果放入之后，缓存项的数量超过了上限，那么就要从队列尾部弹出一个缓存项丢弃掉，并且从Map里也删去这个缓存项的键值对。

这里双端队列的实现，我们可以用 java 提供的 Deque，也可以手动实现一个。用 java 提供的好处是，代码简洁，坏处是性能很慢，因为把队列中的某个元素放到队列头部这个操作需要我们先删掉它，然后把它重新放入头部。java自带的ArrayDeque和LinkedList都会先遍历一遍队列找到对应的元素，会有性能上的损失。我们这里自己实现一个`remove(DLinkedListNode node)`来快速删除。

用自带的 Deque 实现：

```java
class LRUCache {
    private Map<Integer, Item> cache;
    private Deque<Item> usage;
    private int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        cache = new HashMap<>(capacity);
        usage = new ArrayDeque<>();
    }

    public int get(int key) {
        Item item = cache.get(key);
        if (item == null) {
            return -1;
        }
        usage.remove(item);
        usage.offerFirst(item);
        return item.value;
    }

    public void put(int key, int value) {
        Item item = cache.get(key);
        if (item != null) {
            item.value = value;
            usage.remove(item);
            usage.offerFirst(item);
        } else {
            item = new Item(key, value);
            cache.put(key, item);
            usage.offerFirst(item);
            if (cache.size() > this.capacity) {
                cache.remove(usage.pollLast().key);
            }
        }
    }

    class Item {
        int key;
        int value;

        Item(int k, int v) {
            this.key = k;
            this.value = v;
        }
    }
}
```

自己实现个双端队列，速度快了很多。

```java
class LRUCache {
    private Map<Integer, DNode> cache;
    private DLinkedList usage;
    private int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        cache = new HashMap<>(capacity);
        usage = new DLinkedList();
    }

    public int get(int key) {
        DNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        usage.moveToFirst(node);
        return node.value;
    }

    public void put(int key, int value) {
        DNode node = cache.get(key);
        if (node != null) {
            node.value = value;
            usage.moveToFirst(node);
        } else {
            node = new DNode(key, value);
            cache.put(key, node);
            usage.offerFirst(node);
            if (cache.size() > this.capacity) {
                cache.remove(usage.pollLast().key);
            }
        }
    }
}

class DLinkedList {
    private DNode head;
    private DNode tail;

    DLinkedList() {
        head = new DNode(-1, -1);
        tail = new DNode(-1, -1);
        head.next = tail;
        tail.prev = head;
    }

    public void offerFirst(DNode node) {
        node.next = head.next;
        node.next.prev = node;
        head.next = node;
        node.prev = head;
    }

    public DNode pollLast() {
        DNode last = tail.prev;
        this.removeNode(last);
        return last;
    }

    private void removeNode(DNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
        node.next = null;
        node.prev = null;
    }

    public void moveToFirst(DNode node) {
        this.removeNode(node);
        this.offerFirst(node);
    }
}

class DNode {
    int key;
    int value;
    DNode prev;
    DNode next;

    DNode(int key, int val) {
        this.key = key;
        this.value = val;
    }
}
```