# 692. Top K Frequent Words

给定一个数组的字符串，输出出现频率最高的k个字符串。

这题用Hash表配合小顶堆来解决。首先，我们要先得到每个单词出现的频率。得到频率的哈希表之后，我们把哈希表的每一个元素放入小顶堆，小顶堆的比较条件是
+ 如果两个单词频率不一样，那就比较频率
+ 如果两个单词频率一样，那就字典序大的反而小，因为频率一样的话，字典序小的要排在前面。

当小顶堆不足k个元素时，持续加入哈希表的元素。当小顶堆已经有了k个元素，而且当前元素的频率大于等于堆顶的元素频率时，先放入堆中，再弹出堆顶。这样我们就可以时刻保持堆里是目前为止频率最高的k个单词，频率相同的字典序小的会尽量被留在堆里。

最后，我们依次弹出堆顶元素到列表里，并把列表反转一下，就是从高到低的top k了。

Time complexity: O(n + nlogk), n for get frequency map, poll and offer operation is logk.

Space complexity: O(n + k), n for hash map and k for heap.

```java
class Solution {
  public List<String> topKFrequent(String[] words, int k) {
    List<String> res = new ArrayList<>();
    if (words == null || words.length == 0 || k == 0) {
      return res;
    }
    Map<String, Integer> freqMap = getFreqMap(words);
    PriorityQueue<Map.Entry<String, Integer>> minHeap =
        new PriorityQueue<>(
            k,
            new Comparator<>() {
              public int compare(Map.Entry<String, Integer> e1, Map.Entry<String, Integer> e2) {
                if (e1.getValue().equals(e2.getValue())) {
                  return e2.getKey().compareTo(e1.getKey());
                }
                return e1.getValue().compareTo(e2.getValue());
              }
            });
    for (Map.Entry<String, Integer> entry : freqMap.entrySet()) {
      if (minHeap.size() < k) {
        minHeap.offer(entry);
      } else {
        if (entry.getValue() >= minHeap.peek().getValue()) {
          minHeap.offer(entry);
          minHeap.poll();
        }
      }
    }
    for (int i = 0; i < k; i++) {
      res.add(minHeap.poll().getKey());
    }
    Collections.reverse(res);
    return res;
  }

  private Map<String, Integer> getFreqMap(String[] words) {
    Map<String, Integer> freqMap = new HashMap<>();
    for (String word : words) {
      freqMap.put(word, freqMap.getOrDefault(word, 0) + 1);
    }
    return freqMap;
  }
}
```

2025 code:

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        List<String> res = new ArrayList<>();
        if (words == null || words.length == 0 || k == 0) {
            return res;
        }
        Map<String, Integer> freqMap = getFreqMap(words);
        PriorityQueue<Map.Entry<String, Integer>> minHeap = new PriorityQueue<>((e1, e2) -> {
            int cmp = Integer.compare(e1.getValue(), e2.getValue());
            if (cmp != 0) {
                return cmp;
            } else {
                return e2.getKey().compareTo(e1.getKey());
            }
        });
        for (Map.Entry<String, Integer> entry : freqMap.entrySet()) {
            minHeap.offer(entry);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        for (int i = 0; i < k; i++) {
            res.add(minHeap.poll().getKey());
        }
        Collections.reverse(res);
        return res;
    }

    private Map<String, Integer> getFreqMap(String[] words) {
        Map<String, Integer> freqMap = new HashMap<>();
        for (String w : words) {
            int count = freqMap.getOrDefault(w, 0) + 1;
            freqMap.put(w, count);
        }
        return freqMap;
    }
}
```