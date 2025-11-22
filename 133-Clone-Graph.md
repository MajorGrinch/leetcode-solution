# 133. Clone Graph

给定一个连通的无向图，把它deep copy一份出来。

## BFS Approach

广度优先搜索方法，核心思想是利用广度优先搜索，在搜索的过程中对整张图进行复制。用队列来存放需要复制的节点，用`oldNewMap`来记录新老图里的节点对应关系，以及这个节点有没有被复制过。

每从队头拿出节点处理时，先得到它在新图里对应的节点。但是此时不一定所有的邻居都没被复制过，只有不在`oldNewMap`里的邻居没有被复制过，需要入队进行复制。所以说，如果一个邻居已经在`oldNewMap`里了，那么我们之前获取它在新图对应的节点加进来。否则，就进行复制加入`oldNewMap`，然后该邻居入队等待被复制。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return node;
        }
        Map<Node, Node> oldNewMap = new HashMap<>();
        oldNewMap.put(node, new Node(node.val));
        Queue<Node> queue = new ArrayDeque<>();
        queue.offer(node);
        while (!queue.isEmpty()) {
            Node curr = queue.poll();
            Node currCopy = oldNewMap.get(curr);
            for (Node nei : curr.neighbors) {
                if (!oldNewMap.containsKey(nei)) {
                    oldNewMap.put(nei, new Node(nei.val));
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

这题也可以用深度优先搜索，我们实现一个`cloneNode`方法来进行深度优先搜索，在过程中进行拷贝。

如果一个点已经出现在`oldNewMap`里了，直接返回它新图里对应点。如果没有出现过，那就新建并且把映射关系确定。但此时这个点并没有完全被复制好，我们还需要把它的邻居也复制好。所以针对每个邻居，我们进行相同的方法调用，把邻居都复制好之后，返回该新点在新图里的点。

Time complexity: O(E). E is the number of edges.

Space complexity: O(N). N levels call stack.

```java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return node;
        }
        return copyNode(node, new HashMap<>());
    }

    private Node copyNode(Node node, Map<Node, Node> oldNewMap) {
        if (oldNewMap.containsKey(node)) {
            return oldNewMap.get(node);
        }
        Node nodeCopy = new Node(node.val);
        oldNewMap.put(node, nodeCopy);
        for (Node nei : node.neighbors) {
            nodeCopy.neighbors.add(copyNode(nei, oldNewMap));
        }
        return nodeCopy;
    }
}
```