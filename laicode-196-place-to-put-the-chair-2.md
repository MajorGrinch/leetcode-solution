# Laicode 196. Place To Put The Chair II

和[laicode 195](laicode-195-Place-To-Put-Chair-I.md)类似，区别在于没有障碍物点，而且所有格子都可以放椅子。

```java
public class Solution {
  public List<Integer> putChair(char[][] gym) {
    List<Integer> res = Arrays.asList(-1, -1);
    int m = gym.length;
    int n = gym[0].length;
    int[][] dis = new int[m][n];
    for(int x = 0; x < m; x++) {
      for(int y = 0; y < n; y++) {
        if(gym[x][y] == 'E') {
          mergeDistance(dis, getDistance(gym, x, y));
        }
      }
    }
    int cost = Integer.MAX_VALUE;
    for(int x = 0; x < m; x++) {
      for(int y = 0; y < n; y++) {
        if(cost > dis[x][y]) {
          cost = dis[x][y];
          res.set(0, x);
          res.set(1, y);
        }
      }
    }
    return res;
  }

  private void mergeDistance(int[][] dis1, int[][] dis2) {
    for(int i = 0; i < dis1.length; i++) {
      for(int j = 0; j < dis1[0].length; j++) {
        dis1[i][j] += dis2[i][j];
      }
    }
  }

  private int[][] getDistance(char[][] gym, int startX, int startY) {
    int m = gym.length;
    int n = gym[0].length;
    Queue<Integer> qX = new ArrayDeque<>();
    Queue<Integer> qY = new ArrayDeque<>();
    boolean[][] vis = new boolean[m][n];
    int[][] dis = new int[m][n];
    vis[startX][startY] = true;
    dis[startX][startY] = 0;
    qX.offer(startX);
    qY.offer(startY);
    int[] dx = {-1, 1, 0, 0};
    int[] dy = {0, 0, -1, 1};
    while(!qX.isEmpty()) {
      int x = qX.poll();
      int y = qY.poll();
      for(int i = 0; i < dx.length; i++) {
        int nextX = x + dx[i];
        int nextY = y + dy[i];
        if(0 <= nextX && nextX < m && 0 <= nextY && nextY < n && !vis[nextX][nextY]) {
          qX.offer(nextX);
          qY.offer(nextY);
          vis[nextX][nextY] = true;
          dis[nextX][nextY] = dis[x][y] + 1;
        }
      }
    }
    return dis;
  }
}
```