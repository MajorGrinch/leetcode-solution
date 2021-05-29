# Laicode 682. Longest Ascending Subsequence II

## Dynamic Programming Approach

和[300. Longest Increasing Subsequence](300-Longest-Increasing-Subsequence.md)差不多，但是我们要返回那个最长的序列，而不是长度。`lis[i]`记录的是以`i`下标结尾的最长升序序列长度，我们用一个`prev[i]`来记录以`i`下标结尾的这个最长升序序列的前一个元素的下标。

`prev[i]`全体初始化为`-1`。那么很显然，`lis[i] = MAX(lis[j]) + 1, j < i && a[j] < a[i]`，所以`prev[i]`也就是那个最大的满足条件的`lis[j]`的`j`。如果没有满足的`j`，那么`prev[i]`还是为`-1`。同时我们也记录一下以哪个`i`结尾的最长升序序列是整个数组的最长升序序列，然后通过`prev`来回溯。

Time complexity: O(n^2)

Space complexity: O(n)

```java
public class Solution {
  public int[] longest(int[] a) {
    if (a.length <= 1) {
      return a;
    }

    int n = a.length;
    int[] lis = new int[n];
    int[] prev = new int[n];
    int longestLen = 1;
    int longestEndingIdx = 0;
    lis[0] = 1;
    Arrays.fill(prev, -1);
    for (int i = 1; i < n; i++) {
      int localLongest = 0;
      for (int j = 0; j < i; j++) {
        if (a[j] < a[i] && lis[j] > localLongest) {
          localLongest = lis[j];
          prev[i] = j;
        }
      }
      localLongest++;
      if (localLongest > longestLen) {
        longestLen = localLongest;
        longestEndingIdx = i;
      }
      lis[i] = localLongest;
    }
    int[] res = new int[longestLen];
    for (int i = longestEndingIdx; i >= 0; i = prev[i]) {
      res[--longestLen] = a[i];
    }
    return res;
  }
}
```

## Binary Search Approach

和[300. Longest Increasing Subsequence](300-Longest-Increasing-Subsequence.md)对应的二分搜索方法差不多，我们依然用`prev[i]`来记录以`i`下标指向的元素为末端元素的最长升序序列的前一个元素下标。但是`lowestEnding[len] = val`记录的是目前长度为`len`的所有升序序列中最小的末端元素是`val`，它并不提供`val`到底是哪个下标这个信息。所以我们用`Map`来记录目前遇到的所有元素对应的下标，放在`idxMap`里。

注意，`idxMap`一定要一步一步放，因为输入里可能有重复元素。如果我们一下子先计算好了，那么比如当前元素之前有个`10`和之后有个`10`，那么这时候就不太好区分了。什么？一开始计算`idxMap`时候不覆盖？小伙纸太年轻了嗷。如果一切元素都只记录第一次出现的下标位置的话，万一相同的元素后一个可以形成更长的升序序列怎么办？比如`3, 1, 2 ,3`。可能这个数组里有多个`val`元素，但是我们需要的是`lowestEnding[len] = val`里面那个序列的末端`val`的位置。一步一步计算`idxMap`的话，假使我们当前得到了`lasLenBf`然后`lowestEnding[lasLenBf] = val`，那么`val`肯定是当前元素之前的一个升序序列的末端元素，`idxMap`也就能得到它确切的位置。

因此，对于`prev[i]`来说，如果`i`元素前面有末端元素比它小的升序序列的话，`prev[i]`自然是等于那个升序序列末端元素的下标。如果没有的话，`prev[i]`就等于`-1`。怎么判断有没有？当然是看`lasLenBf > 0`啦。

最后，我们只要拿到全剧最长的升序序列长度`lasLen`，然后`lowestEnding[lasLen]`得到最长升序序列的末端元素，再通过`idxMap`得到它的下标。什么？担心这个值的下标被之后相同的元素覆盖？仔细想一想之后即使有相同的元素覆盖了，会有影响么？之后的相同元素被带入计算的时候，会是怎么个过程，好好想想。得到下标之后，通过`prev`来回溯，直到`-1`为止，就可以得到这个升序序列的所有元素了。

Time complexity: O(nlogn)

Space complexity: O(n)

```java
public class Solution {
  public int[] longest(int[] a) {
    if (a.length <= 1) {
      return a;
    }

    int n = a.length;
    int[] lowestEnding = new int[n + 1];
    int[] prev = new int[n];
    Map<Integer, Integer> idxMap = new HashMap<>();

    int lasLen = 1;
    lowestEnding[1] = a[0];
    prev[0] = -1;
    idxMap.put(a[0], 0);
    for (int i = 1; i < n; i++) {
      idxMap.put(a[i], i);
      int lasLenBf = largestSmaller(lowestEnding, 1, lasLen, a[i]);
      lowestEnding[lasLenBf + 1] = a[i];
      lasLen = Math.max(lasLen, lasLenBf + 1);
      prev[i] = lasLenBf > 0 ? idxMap.get(lowestEnding[lasLenBf]) : -1;
    }
    int[] res = new int[lasLen];
    int k = lasLen;
    for (int i = idxMap.get(lowestEnding[lasLen]); i >= 0; i = prev[i]) {
      res[--k] = a[i];
    }
    return res;
  }

  private int largestSmaller(int[] array, int l, int r, int target) {
    while (r - l > 1) {
      int mid = l + (r - l >> 1);
      if (array[mid] < target) l = mid;
      else r = mid - 1;
    }
    if (array[r] < target) return r;
    else if (array[l] < target) return l;
    else return l - 1;
  }
}
```