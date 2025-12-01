# Laicode 217. Largest Set Of Points With Positive Slope

给定一堆二维坐标，要求找到最大的子集使得子集内部任意两点之间连线斜率都是正数。

看着复杂，其实就是把输入的坐标按照x轴排序，然后找到最长升序序列。不过需要注意的是，垂直的线不属于正斜率，所以计算的时候需要排除两个点 x 相同的情况。

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

2025 code:

```java
public class Solution {
  public int largest(Point[] points) {
    int n = points.length;
    if(n <= 1) {
      return 0;
    }
    Arrays.sort(points, new Comparator<Point>() {
      @Override
      public int compare(Point p1, Point p2) {
        if(p1.x != p2.x) {
          return Integer.compare(p1.x, p2.x);
        } else {
          return Integer.compare(p1.y, p2.y);
        }
      }
    });
    int[] las = new int[n];
    int res = 1;
    las[0] = 1;
    for(int i = 1; i < n; i++) {
      int local = 0;
      for(int j = 0; j < i; j++) {
        if(points[j].x < points[i].x && points[j].y < points[i].y) {
          local = Math.max(local, las[j]);
        }
      }
      las[i] = local + 1;
      res = Math.max(res, las[i]);
    }
    return res > 1 ? res : 0;
  }
}
```