# 1429. First Unique Number

和[laicode 288. First Non-Repeating Character In Stream](laicode-288-First-Non-Repeating-Character-In-Stream.md)一样的，只不过这题是先有一些初始化的数据，所以在constructor里先`add`就好。

```java
class FirstUnique {

  private Deque<Integer> uniqueDeque = new ArrayDeque<>();
  private Map<Integer, Integer> countMap = new HashMap<>();

  public FirstUnique(int[] nums) {
    for (int num : nums) add(num);
  }

  public int showFirstUnique() {
    return uniqueDeque.isEmpty() ? -1 : uniqueDeque.peekFirst();
  }

  public void add(int value) {
    int count = countMap.getOrDefault(value, 0);
    countMap.put(value, ++count);
    if (count == 1) {
      uniqueDeque.offerLast(value);
    } else {
      while (!uniqueDeque.isEmpty() && countMap.get(uniqueDeque.peekFirst()) > 1) {
        uniqueDeque.pollFirst();
      }
    }
  }
}
```

2025 code:

```java
class FirstUnique {
    private Map<Integer, Integer> freqMap;
    private Queue<Integer> unique;

    public FirstUnique(int[] nums) {
        freqMap = new HashMap<>();
        unique = new LinkedList<>();
        for (int num : nums) {
            this.add(num);
        }
    }

    public int showFirstUnique() {
        if (this.unique.isEmpty()) {
            return -1;
        }
        return this.unique.peek();
    }

    public void add(int value) {
        int count = this.freqMap.getOrDefault(value, 0);
        this.freqMap.put(value, ++count);
        if (count == 1) {
            this.unique.offer(value);
        }
        while (!unique.isEmpty() && this.freqMap.get(unique.peek()) > 1) {
            unique.poll();
        }
    }
}
```