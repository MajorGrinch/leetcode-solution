# Laicode 171. Common Elements In Three Sorted Array

和[laicode 652. Common Numbers Of Two Sorted Arrays](laicode-652-Common-Numbers-Of-Two-Sorted-Arrays.md)类似，只不过这里是变成找三个有序数组里的共同元素。

换汤不换药，既然那题是双指针，那么我们这里就三指针同时走。如果三个指针指向的数字相同，那么就加入答案。否则，就从三个里面找到最小的那个让它向后走直到任一指针走到最后。

Time complexity: O(N). N is the total number of elements in three arrays.

Space complexity: Only constant extra space except the result.

```java
public class Solution {
  public List<Integer> common(int[] a, int[] b, int[] c) {
    // sanity check
    List<Integer> res = new ArrayList<>();
    if (a.length == 0 || b.length == 0 || c.length == 0) return res;

    int ai = 0, bi = 0, ci = 0;
    while (ai < a.length && bi < b.length && ci < c.length) {
      if (a[ai] == b[bi] && b[bi] == c[ci]) {
        res.add(a[ai]);
        ai++;
        bi++;
        ci++;
      } else if (a[ai] <= b[bi] && a[ai] <= c[ci]) {
        ai++;
      } else if (b[bi] <= a[ai] && b[bi] <= c[ci]) {
        bi++;
      } else {
        ci++;
      }
    }
    return res;
  }
}
```