# Laicode 195. Place To Put The Chair I

一个健♂身♂房地图由`C`、`O`和`E`组成，`C`代表普通格子，`O`代表障碍物以及`E`代表器材。求出在哪个格子放椅子可以使得椅子到各个器材的距离和最短。

Clarification:
+ 至少有一个`E`
+ 地图不为空且长宽大于0
+ 每个普通格子都有通向每个器材的路
+ 障碍物格子不可通行
+ 只有普通格子可以放椅子

那么这题还是经典的BFS。我们求出每个器材到健身房每个非障碍物点的距离矩阵，把这些距离矩阵合并起来就可以找到那个离所有器材距离和最近的`C`格子。

具体对每个器材进行BFS求距离矩阵的时候需要注意的是，如果下一步是障碍物的话，就把那一格的距离置为`Integer.MAX_VALUE`因为不可通行。但是下一步是其他器材的话，就不能这么做因为可以`E`格子是可以通行的，只是不能放椅子而已。

我们在合并距离矩阵的方法里也有一个小技巧，只看`dis1`矩阵里面对应的距离是不是`Integer.MAX_VALUE`，因为不管是哪个器材的距离矩阵障碍物点的值肯定都是一样的。这样，我们在生成第一个器材距离矩阵的时候，可以直接把障碍物点拷贝到全局距离矩阵里，后面也可以合并剩下来的非障碍物点。省去了对全局距离矩阵的初始化。

记住，只有`C`格子可以放椅子。

Time complexity: O(M * N * E) where M and N is the rows and columns of the input, E is the number of equipment.

Space complexity: O(M * N * E).

```java
public class Solution {
  public List<Integer> putChair(char[][] gym) {
    int m = gym.length, n = gym[0].length;
    int[][] dis = new int[m][n];
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (gym[i][j] == 'E') {
          mergeDistanceMap(dis, getDistanceMap(gym, i, j));
        }
      }
    }
    int chairX = -1, chairY = -1;
    int shortest = Integer.MAX_VALUE;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (gym[i][j] == 'C' && shortest > dis[i][j]) {
          shortest = dis[i][j];
          chairX = i;
          chairY = j;
        }
      }
    }
    return Arrays.asList(chairX, chairY);
  }

  private void mergeDistanceMap(int[][] dis1, int[][] dis2) {
    for (int i = 0; i < dis1.length; i++) {
      for (int j = 0; j < dis1[i].length; j++) {
        if (dis1[i][j] != Integer.MAX_VALUE) {
          dis1[i][j] += dis2[i][j];
        }
      }
    }
  }

  private int[][] getDistanceMap(char[][] gym, int x, int y) {
    int m = gym.length, n = gym[0].length;
    int[][] dis = new int[m][n];
    boolean[][] vis = new boolean[m][n];
    int[] dx = {-1, 1, 0, 0};
    int[] dy = {0, 0, -1, 1};
    Queue<Integer> qx = new ArrayDeque<>(), qy = new ArrayDeque<>();
    vis[x][y] = true;
    qx.offer(x);
    qy.offer(y);
    while (!qx.isEmpty()) {
      int currX = qx.poll(), currY = qy.poll();
      for (int k = 0; k < dx.length; k++) {
        int nextX = currX + dx[k], nextY = currY + dy[k];
        if (nextX < 0 || nextX >= m || nextY < 0 || nextY >= n || vis[nextX][nextY]) continue;
        if (gym[nextX][nextY] == 'O') {
          dis[nextX][nextY] = Integer.MAX_VALUE;
          vis[nextX][nextY] = true;
          continue;
        }
        vis[nextX][nextY] = true;
        dis[nextX][nextY] = dis[currX][currY] + 1;
        qx.offer(nextX);
        qy.offer(nextY);
      }
    }
    return dis;
  }
}
```