# Laicode 651. Common Numbers Of Two Arrays II(Array version)

参照[leetcode 350](350-intersection-of-two-arrays-ii.md)可以有两种方法。

## Two pointer approach

Time complexity: O(nlogn) because of the sorting.

Space complexity: Depends on the sorting algorithm.

```java
public class Solution {
  public List<Integer> common(int[] A, int[] B) {
    Arrays.sort(A);
    Arrays.sort(B);
    int p1 = 0, p2 = 0;
    List<Integer> res = new ArrayList<>();
    while (p1 < A.length && p2 < B.length) {
      if (A[p1] == B[p2]) {
        res.add(A[p1]);
        p1++;
        p2++;
      } else if (A[p1] > B[p2]) {
        p2++;
      } else {
        p1++;
      }
    }
    return res;
  }
}
```

## HashMap approach

还有一种方法，就是用Map记录两个数组里面各个数字出现的频率，然后遍历其中一个的频率表去查找数字在另一个数组频率表里面出现的次数。有共同出现的话，就加入答案。

最后给答案排个序再返回就好。

A has lengh as M, B has len as N, res has len as k

Time complexity: O(M + N + klogk),

Space complexity: O(M + N)

```java
public class Solution {
  public List<Integer> common(int[] A, int[] B) {
    Map<Integer, Integer> freqMapA = getFreqMap(A);
    Map<Integer, Integer> freqMapB = getFreqMap(B);
    List<Integer> res = new ArrayList<>();
    for(Map.Entry<Integer, Integer> entry: freqMapA.entrySet()) {
      int key = entry.getKey();
      int countB = freqMapB.getOrDefault(key, 0);
      int count = Math.min(entry.getValue(), countB);
      for(int i = 0; i < count; i++) {
        res.add(key);
      }
    }
    Collections.sort(res);
    return res;

  }

  private Map<Integer, Integer> getFreqMap(int[] nums) {
    Map<Integer, Integer> freqMap = new HashMap<>();
    for(int n: nums) {
      int count = freqMap.getOrDefault(n, 0) + 1;
      freqMap.put(n, count);
    }
    return freqMap;
  }
}
```