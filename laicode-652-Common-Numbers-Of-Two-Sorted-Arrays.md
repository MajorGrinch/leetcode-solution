# Laicode 652. Common Numbers Of Two Sorted Arrays

给定两个有序数组，找出他们重合的元素。

Clarification:
+ 两个数组是升序还是降序？升序
+ 有无重复元素？有
+ 答案是否要有序？要升序

## Two Pointers Approach

用双指针的思想，p1和p2两个指针各自从A和B的头部出发。
+ A[p1] == B[p2]，说明这个元素是重合元素，两个指针各自自增以向后找。
+ A[p1] > B[p2]，说明p1指向的元素大一些，所以把p2往后挪一步试图赶上p1的元素
+ A[p1] < B[p2]，说明p2指向的元素大一些，所以把p1往后挪一步试图赶上p2的元素。

这样我们最后就可以拿到重合的元素列表。

Time complexity: O(max(M, N)), M and N are length of two arrays respectively.


```java
public class Solution {
  public List<Integer> common(int[] A, int[] B) {
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