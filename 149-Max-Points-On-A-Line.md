# 149. Max Points on a Line

给出一堆点的坐标，求出最大共线的点数。

这题我只能想到用暴力的枚举方法了，双重for循环遍历每个点和其他点的搭配。但是呢，给定的点里会有一些特殊情况比如重复，或者横坐标相等，这些都需要考虑好。

所以给定一个点，遍历剩余点的时候要记录好重复点数，横坐标相同点数以及斜线上不同斜率对应的点数。因为斜率不一定是正数，double可能会丢失精度，所以我们用字符串记录斜率的分数形式来表示斜率。这就要求该分数一定得是不可再分，不然`1/1`和`2/2`本质上是同一个斜率但会被纳入不同的斜率。把横坐标差和纵坐标差除以他们的最大公约数就可以约分得到最简分数。（艹，我竟然还记得约分和最简分数这些概念。。。）

内层循环结束之后，我们就可以得到与当前点重复的点数，同x轴的点数，正常斜率的直线里最多的共线点数。他们三个里面取最大值加一，就是这个点对应的最大共线点数。用他来试图更新答案即可。

需要注意的是，当题目给定的点数不足3个时，我们直接返回给的点数就行了。

Time complexity: O(n^2)

Space complexity: O(n) because of the hash map.

```java
class Solution {
  public int maxPoints(int[][] points) {
    if (points.length <= 2) {
      return points.length;
    }
    int res = 2;
    int n = points.length;
    for (int i = 0; i < n - 1; i++) {
      int overlapped = 0, vertical = 0, sloped = 0;
      Map<String, Integer> slopeCount = new HashMap<>();
      int x1 = points[i][0], y1 = points[i][1];
      for (int j = i + 1; j < n; j++) {
        int x2 = points[j][0], y2 = points[j][1];
        if (x1 == x2) {
          if (y1 == y2) overlapped++;
          else vertical++;
          continue;
        }
        int dx = x1 - x2, dy = y1 - y2;
        int gcdXY = gcd(dx, dy);
        dx /= gcdXY;
        dy /= gcdXY;
        String k = dy + "/" + dx;
        slopeCount.put(k, slopeCount.getOrDefault(k, 0) + 1);
        sloped = Math.max(sloped, slopeCount.get(k));
      }
      int localMax = Math.max(sloped, Math.max(overlapped, vertical)) + 1;
      res = Math.max(res, localMax);
    }
    return res;
  }

  private int gcd(int x, int y) {
    return y == 0 ? x : gcd(y, x % y);
  }
}
```