# Laicode 681. Seven Puzzle

给定一个2X4的棋盘，和对应的棋子摆放。只能移动0，问最少多少步可以把棋子移动成规定的样子。如果怎么也不可能变成规定的样子，就返回-1。

这题就是BFS不断地搜索所有可能的状态，比如`10234567`中的0可以向左、右和下各自走一步，下一个状态分别对应`012345678`、`12034567`和`15234067`。`01234567`是我们需要达到的状态，所以我们知道走一步就到了。

所以这题的大题思想就是，先找到0所在的位置，然后让0向所有可能的地方走。用一个Set来记录目前为止所有遇到的棋盘状态，如果挪动0之后产生的是没有遇过的新状态，就加入Set。如果这个状态以前遇到过，那就不动。这样，我们可以知道0走1步可以得到哪些状态，2步可以得到哪些状态，3步、4步……。棋盘的状态数量肯定是有限的，如果能搜到那肯定能找到所需步数，如果搜不到就返回-1就行。

这题有很多可以优化的地方：
+ 题目输入的是一个数组，但是对比数组相同需要自己实现，而且Set里面放`int[]`达不到我们要的效果。因此，我们把输入的数组序列化为字符串，方便使用。
+ 每次都要找到0所在的位置可能会带来额外时间，所以我用了一个内部类`State`来记录目前的棋盘状态和它里面0所在的位置。
+ 对于每一个位置来说，它能走的地方其实是有限的。所以我们直接hardcode所有位置能走的所有地方，直接调用就好。

```java
public class Solution {
  public int numOfSteps(int[] values) {
    StringBuilder boardBuilder = new StringBuilder();
    for (int i = 0; i < values.length; i++) {
      boardBuilder.append(values[i]);
    }
    Queue<State> queue = new LinkedList<>();
    Set<String> vis = new HashSet<>();
    int[][] nextIndexMap =
        new int[][] {{1, 4}, {0, 2, 5}, {1, 3, 6}, {2, 7}, {0, 5}, {1, 4, 6}, {2, 5, 7}, {3, 6}};

    String initialBoard = boardBuilder.toString();
    State initial = new State(initialBoard, initialBoard.indexOf('0'));
    queue.offer(initial);
    vis.add(initial.board);
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
    String solved = "01234567";
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