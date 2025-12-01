# Laicode 216. Most Points On A Line

参照[leetcode 149](149-Max-Points-On-A-Line.md)，但是这题有略微的区别就是输入的点可能有重复的点。

```java
public class Solution {
  public int most(Point[] points) {
    int n = points.length;
    if(n <= 2) {
      return n;
    }
    int res = 2;
    for(int i = 0; i < n - 1; i++) {
      int overlap = 0;
      int vertical = 0;
      int slopedMax = 0;
      Map<String, Integer> slopedCount = new HashMap<>();
      int x1 = points[i].x;
      int y1 = points[i].y;
      for(int j = i + 1; j < n; j++) {
        int x2 = points[j].x;
        int y2 = points[j].y;
        if(x1 == x2) {
          if(y1 == y2) {
            overlap++;
          } else {
            vertical++;
          }
          continue;
        }
        int dx = x2 - x1;
        int dy = y2 - y1;
        int gcdXY = gcd(dx, dy);
        dx /= gcdXY;
        dy /= gcdXY;
        String k = dy + "/" + dx;
        slopedCount.put(k, slopedCount.getOrDefault(k, 0) + 1);
        slopedMax = Math.max(slopedMax, slopedCount.get(k));
      }
      int maxForI = Math.max(vertical, slopedMax) + overlap + 1;
      res = Math.max(res, maxForI);
    }
    return res;
  }

  private int gcd(int x, int y) {
    return y == 0 ? x : gcd(y, x % y);
  }
}
```