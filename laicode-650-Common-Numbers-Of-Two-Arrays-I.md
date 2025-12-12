# Laicode 650. Common Numbers Of Two Arrays I(Array version)

参照[leetcode 350](350-intersection-of-two-arrays-ii.md)。

Time complexity: O(nlogn) because of the sorting process.

Space complexity: Depends on the sorting algorithm. Quick sort takes O(logn) in average, and O(n) in worst case. Merge sort takes O(n) all the time.

```java
public class Solution {
  public List<Integer> common(int[] a, int[] b) {
    Arrays.sort(a);
    Arrays.sort(b);
    int p1 = 0, p2 = 0;
    List<Integer> res = new ArrayList<>();
    while (p1 < a.length && p2 < b.length) {
      if (a[p1] == b[p2]) {
        res.add(a[p1]);
        p1++;
        p2++;
      } else if (a[p1] > b[p2]) {
        p2++;
      } else {
        p1++;
      }
    }
    return res;
  }
}
```