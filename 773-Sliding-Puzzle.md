# 773. Sliding Puzzle

和[laicode 681. Seven Puzzle](laicode-681-Seven-Puzzle.md)几乎一样，不多赘述。

```java
class Solution {
  public int slidingPuzzle(int[][] puzzle) {
    StringBuilder boardBuilder = new StringBuilder();
    for (int i = 0; i < puzzle.length; i++) {
      for (int j = 0; j < puzzle[i].length; j++) {
        boardBuilder.append(puzzle[i][j]);
      }
    }
    Queue<State> queue = new LinkedList<>();
    Set<String> vis = new HashSet<>();
    int[][] nextIndexMap = new int[][] {{1, 3}, {0, 2, 4}, {1, 5}, {0, 4}, {1, 3, 5}, {2, 4}};

    String initialBoard = boardBuilder.toString();
    State initial = new State(initialBoard, initialBoard.indexOf('0'));
    queue.offer(initial);
    vis.add(initialBoard);
    int steps = 0;
    while (!queue.isEmpty()) {
      int size = queue.size();
      for (int i = 0; i < size; i++) {
        State curr = queue.poll();
        if (isSolved(curr.board)) return steps;
        for (int nextIndex : nextIndexMap[curr.zeroIndex]) {
          String nextBoard = swap(curr.board, curr.zeroIndex, nextIndex);
          boolean unvisited = vis.add(nextBoard);
          if (!unvisited) continue;
          queue.offer(new State(nextBoard, nextIndex));
        }
      }
      steps++;
    }

    return -1;
  }

  private boolean isSolved(String state) {
    String solved = "123450";
    return state.equals(solved);
  }

  private String swap(String s, int i, int j) {
    StringBuilder sb = new StringBuilder(s);
    sb.setCharAt(i, s.charAt(j));
    sb.setCharAt(j, s.charAt(i));
    return sb.toString();
  }

  class State {
    String board;
    int zeroIndex;

    State(String board, int zeroIndex) {
      this.board = board;
      this.zeroIndex = zeroIndex;
    }
  }
}
```