# Laicode 132. Deep Copy Undirected Graph

这题主要参照[leetcode 133](133-Clone-Graph.md)，但是这题有个最重要的 assumption 就是不保证是连通图。所以我们针对每个节点，都要去单独进行复制，防止各个点都是互不相连的（当然了，这是最糟情况）。

同理，这里有 BFS 和 DFS 两种路线。

## BFS Approach

```java
public class Solution {
  public List<GraphNode> copy(List<GraphNode> graph) {
    List<GraphNode> graphCopy = new ArrayList<>();
    for(GraphNode n: graph) {
      graphCopy.add(copyNode(n, new HashMap<>()));
    }
    return graphCopy;
  }

  private GraphNode copyNode(GraphNode node, Map<GraphNode, GraphNode> oldNewMap) {
    if(oldNewMap.containsKey(node)) {
      return oldNewMap.get(node);
    }
    oldNewMap.put(node, new GraphNode(node.key));
    Queue<GraphNode> queue = new ArrayDeque<>();
    queue.offer(node);
    while(!queue.isEmpty()) {
      GraphNode curr = queue.poll();
      GraphNode currCopy = oldNewMap.get(curr);
      for(GraphNode nei: curr.neighbors) {
        if(!oldNewMap.containsKey(nei)) {
          oldNewMap.put(nei, new GraphNode(nei.key));
          queue.offer(nei);
        }
        currCopy.neighbors.add(oldNewMap.get(nei));
      }
    }
    return oldNewMap.get(node);
  }
}
```

## DFS Approach

```java
public class Solution {
  public List<GraphNode> copy(List<GraphNode> graph) {
    List<GraphNode> graphCopy = new ArrayList<>();
    for(GraphNode n: graph) {
      graphCopy.add(copyNode(n, new HashMap<>()));
    }
    return graphCopy;
  }

  private GraphNode copyNode(GraphNode node, Map<GraphNode, GraphNode> oldNewMap) {
    if(oldNewMap.containsKey(node)) {
      return oldNewMap.get(node);
    }
    GraphNode nodeCopy = new GraphNode(node.key);
    oldNewMap.put(node, nodeCopy);
    for(GraphNode nei: node.neighbors) {
      nodeCopy.neighbors.add(copyNode(nei, oldNewMap));
    }
    return nodeCopy;
  }
}
```