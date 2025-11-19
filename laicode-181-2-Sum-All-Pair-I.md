# Laicode 181. 2 Sum All Pair I

还是two sum，不过答案不唯一，要找出所有答案。

这里我们的`idxMap`不再记录某个数出现在某个下标，而是出现在某一系列下标。因为题目不保证没有重复元素，所以如果有重复元素满足答案的话，肯定需要都记录下来。

每轮循环先看看`idxMap`里有没有`target - array[i]`，有的话得到所有对应的下标和当前的`i`加入答案。然后记录`array[i]`出现在`i`下标再结束本轮循环。

Time complexity: O(N)

Space complexity: O(N)

```java
public class Solution {
  public List<List<Integer>> allPairs(int[] array, int target) {
    List<List<Integer>> res = new ArrayList<>();
    Map<Integer, List<Integer>> idxMap = new HashMap<>();
    for (int i = 0; i < array.length; i++) {
      List<Integer> anotherIndexes =
          idxMap.getOrDefault(target - array[i], Collections.emptyList());
      for (int idx : anotherIndexes) res.add(Arrays.asList(idx, i));
      idxMap.putIfAbsent(array[i], new ArrayList<>());
      idxMap.get(array[i]).add(i);
    }
    return res;
  }
}
```

2025 code:

```java
public class Solution {
  public List<List<Integer>> allPairs(int[] array, int target) {
    Map<Integer, List<Integer>> idxMap = new HashMap<>();
    List<List<Integer>> res = new ArrayList<>();
    for(int i = 0; i < array.length; i++) {
      int v1 = array[i];
      int v2 = target - v1;
      if(idxMap.containsKey(v2)) {
        for(int idx: idxMap.get(v2)) {
          res.add(Arrays.asList(idx, i));
        }
      }
      idxMap.putIfAbsent(v1, new ArrayList<>());
      idxMap.get(v1).add(i);
    }
    return res;
  }
}
```