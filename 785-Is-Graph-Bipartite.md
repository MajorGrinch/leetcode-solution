# 785. Is Graph Bipartite

判断一个图是否是二部图，所谓二部图就是说这个图是否可以分成两组，组内节点互不相连，这个图所有的边都是跨越这两个组之间的。

## BFS Approach

这题用宽度优先搜索配合染色，用vis数组来记录一个点是否被访问过以及它的颜色。BFS的时候，分两种情况，
+ 邻居没有被访问过，那么把邻居染成和当前点不同的颜色
+ 邻居被访问过，那么看看邻居是不是颜色和自己不一样。一样的话，这就不是二部图了。

此外，题目有说过这个图不一定是连通的，所以我们要针对每个点都进行一次BFS。如果该点被访问过了，那就不用搜。如果没被访问过，那就搜下去。

Time complexity: O(N + E), N is the node num and E is the edge num.

Space complexity: O(N), because BFS needs a queue which can hold up to all nodes of this graph.

```java
class Solution {
  public boolean isBipartite(int[][] graph) {
    if (graph == null || graph.length == 0) {
      return true;
    }
    int[] vis = new int[graph.length];
    Arrays.fill(vis, 0);
    for (int i = 0; i < graph.length; i++) {
      if (!BFS(graph, i, vis)) {
        return false;
      }
    }
    return true;
  }

  private boolean BFS(int[][] graph, int k, int[] vis) {
    if (vis[k] != 0) {
      return true;
    }
    Queue<Integer> queue = new ArrayDeque<>();
    queue.offer(k);
    vis[k] = 1;
    while (!queue.isEmpty()) {
      int curr = queue.poll();
      int color = vis[curr] == 1 ? -1 : 1;
      for (int n : graph[curr]) {
        if (vis[n] == 0) {
          vis[n] = color;
          queue.offer(n);
        } else if (vis[n] != color) {
          return false;
        }
      }
    }
    return true;
  }
}
```

## DFS Approach

这题也可以用深度优先遍历配合染色，思路和BFS的一样。遇到邻居如果没访问过，那就染上和自己不一样的颜色，并进行搜索。如果访问过，那就对比颜色。

因为图不一定是连通的，所以要对每个未访问过的点都进行一次深搜，搜之前先染上默认的颜色。一次DFS就可以把整个连通分量里的点都染上色，所以不用担心重复染色的问题，即每个点只会被染色一次。

Time complexity: O(N + E), because we will have to traverse every node only once and every edge once once.

Space complexity: O(N), because dfs will cause at most N level call stack.

```java
class Solution {
  public boolean isBipartite(int[][] graph) {
    if (graph == null || graph.length == 0) {
      return true;
    }
    int[] vis = new int[graph.length];
    Arrays.fill(vis, 0);
    for (int i = 0; i < graph.length; i++) {
      if(vis[i] != 0){
        continue;
      }else{
        vis[i] = 1;
      }
      if (!DFS(graph, i, vis)) {
        return false;
      }
    }
    return true;
  }

  private boolean DFS(int[][] graph, int k, int[] vis) {
    for(int n: graph[k]){
      if(vis[n] == 0){
        vis[n] = vis[k] == 1 ? -1 : 1;
        if(!DFS(graph, n, vis)){
          return false;
        }
      }else if(vis[n] == vis[k]){
        return false;
      }
    }
    return true;
  }
}
```