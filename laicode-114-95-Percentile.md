# Laicode 114. 95 Percentile

给定一个列表的URL长度，找出第95%大的长度。

像这种P95或者是P90、P99和P50的题目都可以用一个省空间的方法来做。题目说了，URL长度最长也就是4096，所以我们可以开个长度为4097的int数组来记录各个长度出现了多少次。然后最大长度开始往前遍历，如果当前累积的个数已经超过%5的元素总个数的话，那么当前的长度就是P95，可以直接作为答案返回。

Time complexity: O(N)

Space complexity: O(1)，因为只和输入数据的范围有关，而不和输入数据的规模有关。

```java
public class Solution {
  public int percentile95(List<Integer> lengths) {
    if (lengths == null || lengths.size() == 0) {
      return 0;
    }
    int[] lenCount = new int[4097];
    int n = lengths.size();
    double p05 = n * 0.05;
    for (int len : lengths) lenCount[len]++;
    int currAmount = 0;
    for (int len = 4096; len >= 0; len--) {
      currAmount += lenCount[len];
      if (currAmount > p05) {
        return len;
      }
    }
    return -1;
  }
}
```