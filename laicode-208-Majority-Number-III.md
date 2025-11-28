# Laicode 208. Majority Number III

这题就是更加通用的情况了，给定一个数组，Majority Number定义为出现超过`1/k`次就好。

那么我们这题用一个`Map<Integer, Integer> counter`来记录每个candidiate出现过多少次。遍历数组的时候，首先检查当前数字是否在`counter`里。如果在的话，把对应的次数加一。如果不在的话，还得看`counter`里的候选数字数量是否有`k-1`个。如果不足的话，就把它当成新的候选数字加入。如果已经满`k-1`个，那就所有的候选数字对应的count都要减一。不过这里要注意，如果减一之后变成0的话，就可以直接从counter里删除掉了，以免这个数占着 Map 的坑位。然后在实现的时候，一定注意在keySet基础上新建一个ArrayList再访问，不然会有`ConcurrentModificationException`。

Time complexity: O(N) in average. Even if we have to iterate over the map to subtract the frequency, the total number of remove operation is less than the total number of put operation. So it's O(N).

Spaec complexity: O(k) because of the map.

```java
public class Solution {
  public List<Integer> majority(int[] array, int k) {
    Map<Integer, Integer> counter = new HashMap<>();
    for (int num : array) {
      if (counter.containsKey(num)) {
        counter.put(num, counter.get(num) + 1);
      } else {
        if (counter.size() < k - 1) {
          counter.put(num, 1);
        } else {
          List<Integer> keyList = new ArrayList<>(counter.keySet());
          for (int key : keyList) {
            int count = counter.get(key);
            if (count == 1) counter.remove(key);
            else counter.put(key, count - 1);
          }
        }
      }
    }

    List<Integer> majorityList = new ArrayList<>();
    Map<Integer, Integer> freqMap = new HashMap<>();
    for (int num : array) {
      if (counter.containsKey(num)) {
        freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
      }
    }
    for (int num : freqMap.keySet()) {
      int count = freqMap.get(num);
      if (count > array.length / k) majorityList.add(num);
    }
    return majorityList;
  }
}
```

2025 code:

```java
public class Solution {
  public List<Integer> majority(int[] array, int k) {
    Map<Integer, Integer> candidateMap = new HashMap<>();
    for(int num: array) {
      if(candidateMap.containsKey(num)) {
        candidateMap.put(num, candidateMap.get(num) + 1);
      } else {
        if(candidateMap.size() < k - 1) {
          candidateMap.put(num, 1);
        } else {
          List<Integer> candidateList = new ArrayList<>(candidateMap.keySet());
          for(int candidate: candidateList) {
            int count = candidateMap.get(candidate);
            count--;
            if(count == 0) {
              candidateMap.remove(candidate);
            } else {
              candidateMap.put(candidate, count);
            }
          }
        }
      }
    }
    Map<Integer, Integer> freqMap = new HashMap<>();
    for(int num: array) {
      if(candidateMap.keySet().contains(num)) {
        freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
      }
    }
    List<Integer> res = new ArrayList<>();
    for(int num: freqMap.keySet()) {
      if(freqMap.get(num) > array.length / k) {
        res.add(num);
      }
    }
    return res;
  }
}
```