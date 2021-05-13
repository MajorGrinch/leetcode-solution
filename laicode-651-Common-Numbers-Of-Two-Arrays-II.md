# Laicode 651. Common Numbers Of Two Arrays II(Array version)

这题还是和[laicode 652. Common Numbers Of Two Sorted Arrays](laicode-652-Common-Numbers-Of-Two-Sorted-Arrays.md)差不多，只不过是无序。先排序，后用双指针。

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