# Laicode 182. 2 Sum All Pair II

还是two sum，但是不返回下标组合而是返回所有的元素组合，并且不能有重复。

这题和之前的two sum不太一样，返回元素组合不说，还兼具去重。那么我们就不能用元素到下标的映射了，而是元素到出现次数的映射。分情况讨论，
+ 当前元素正好是`target`的一半时，那么只要当前元素之前出现过一次，那么这就是个组合。
+ 当前元素不是`target`的一半时，只要`target - array[i]`出现过并且当前`array[i]`没有出现过，那么我们就可以保证这个组合没有用过。加入答案。

结束时记得更新`array[i]`出现的次数。

Time complexity: O(N)

Space complexity: O(N)

```java
public class Solution {
  public List<List<Integer>> allPairs(int[] array, int target) {
    List<List<Integer>> res = new ArrayList<>();
    Map<Integer, Integer> numCount = new HashMap<>();
    for (int num : array) {
      int count = numCount.getOrDefault(num, 0);
      if (target - num == num) {
        if (count == 1) {
          res.add(Arrays.asList(num, num));
        }
      } else {
        if (numCount.getOrDefault(target - num, 0) != 0 && count == 0) {
          res.add(Arrays.asList(num, target - num));
        }
      }
      numCount.put(num, count + 1);
    }
    return res;
  }
}
```

2025 code:

```java
public class Solution {
  public List<List<Integer>> allPairs(int[] array, int target) {
    Map<Integer, Integer> freqMap = new HashMap<>();
    List<List<Integer>> res = new ArrayList<>();
    for(int num: array) {
      int count = freqMap.getOrDefault(num, 0);
      if(target - num == num && count == 1) {
        res.add(Arrays.asList(num, num));
      } else if(target - num != num && freqMap.containsKey(target - num) && count == 0) {
        res.add(Arrays.asList(target - num, num));
      }
      freqMap.put(num, count + 1);
    }
    return res;
  }
}
```