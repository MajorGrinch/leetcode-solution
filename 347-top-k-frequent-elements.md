# 347. Top K Frequent Elements

经典Top K大，就用小顶堆来解决。先遍历一遍数组，获得每个元素的出现次数。小顶堆根据元素次数来排序，元素和对应的次数依次入堆。当堆的个数大于K的时候，就弹出堆顶。

这样剩下来就是K个最频繁的元素，写进结果。

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        PriorityQueue<Map.Entry<Integer, Integer>> minHeap = new PriorityQueue<>(
                (e1, e2) -> Integer.compare(e1.getValue(), e2.getValue()));
        Map<Integer, Integer> freqMap = getFreqMap(nums);
        for (Map.Entry<Integer, Integer> entry : freqMap.entrySet()) {
            minHeap.offer(entry);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        int[] topK = new int[k];
        for (int i = 0; i < k; i++) {
            topK[i] = minHeap.poll().getKey();
        }
        return topK;
    }

    private Map<Integer, Integer> getFreqMap(int[] nums) {
        Map<Integer, Integer> freqMap = new HashMap();
        for (int num : nums) {
            int freq = freqMap.getOrDefault(num, 0) + 1;
            freqMap.put(num, freq);
        }
        return freqMap;
    }
}
```