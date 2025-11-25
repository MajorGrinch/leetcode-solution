# Laicode 218. Generate Random Maze

生成一个迷宫，要求所有空格到出口有且只有一条路，并且空格要尽可能的多。

其实很简单，就是一次跨两步，同时把这两格都变成 0。变成 0 之后，我们再继续选择一个方向跨两步变成 0，如此一直递归调用下去就可以生成一个有且只有一条通路的迷宫。

```java
public class Solution {
  public int[][] maze(int n) {
    int[][] res = new int[n][n];
    for(int i = 0; i < n; i++) {
      for(int j = 0; j < n; j++) {
        res[i][j] = 1;
      }
    }
    res[0][0] = 0;
    generate(res, 0, 0);
    return res;
  }

  private void generate(int[][] maze, int x, int y) {
    Direction[] dirs = Direction.values();
    shuffle(dirs);
    for(Direction d: dirs) {
      int nextX = d.moveX(x, 2);
      int nextY = d.moveY(y, 2);
      if(isWall(maze, nextX, nextY)) {
        maze[d.moveX(x, 1)][d.moveY(y, 1)] = 0;
        maze[nextX][nextY] = 0;
        generate(maze, nextX, nextY);
      }
    }
  }

  private void shuffle(Direction[] dirs) {
    for(int i = 0; i < dirs.length; i++) {
      int index = i + (int)(Math.random() * (dirs.length - i));
      swap(dirs, i, index);
    }
  }

  private void swap(Direction[] dirs, int i, int j) {
    Direction temp = dirs[i];
    dirs[i] = dirs[j];
    dirs[j] = temp;
  }

  private boolean isWall(int[][] maze, int x, int y) {
    return 0 <= x && x < maze.length && 0 <= y && y < maze[0].length && maze[x][y] == 1;
  }
}

enum Direction {
  UP(-1, 0),
  DOWN(1, 0),
  LEFT(0, -1),
  RIGHT(0, 1);

  int deltaX;
  int deltaY;
  
  Direction(int deltaX, int deltaY) {
    this.deltaX = deltaX;
    this.deltaY = deltaY;
  }

  public int moveX(int x, int steps) {
    return x + steps * this.deltaX;
  }

  public int moveY(int y, int steps) {
    return y + steps * this.deltaY;
  }
}
```