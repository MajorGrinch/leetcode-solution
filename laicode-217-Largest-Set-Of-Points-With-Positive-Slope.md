# Laicode 217. Largest Set Of Points With Positive Slope

给定一堆二维坐标，要求找到最大的子集使得子集内部任意两点之间连线斜率都是正数。

看着复杂，其实就是把输入的坐标按照x轴排序，然后找到最长升序序列。

Time complexity: O(nlogn + n^2) = O(n^2)

Space complexity: O(n)

```java
public class Solution {
  public int largest(Point[] points) {
    if (points.length == 0) {
      return 0;
    }

    Arrays.sort(
        points,
        new Comparator<Point>() {
          @Override
          public int compare(Point p1, Point p2) {
            if (p1.x == p2.x) {
              return Integer.compare(p1.y, p2.y);
            }
            return Integer.compare(p1.x, p2.x);
          }
        });
    int n = points.length;
    int[] las = new int[n];

    las[0] = 1;
    int lasLen = 1;
    for (int i = 1; i < n; i++) {
      int localLongest = 0;
      for (int j = 0; j < i; j++) {
        if (points[j].x < points[i].x && points[j].y < points[i].y && las[j] > localLongest) {
          localLongest = las[j];
        }
      }
      las[i] = localLongest + 1;
      lasLen = Math.max(lasLen, las[i]);
    }
    return lasLen == 1 ? 0 : lasLen;
  }
}
```