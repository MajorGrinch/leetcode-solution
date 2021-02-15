# 102. Binary Tree Level Order Traversal

对二叉树进行层序遍历，一层一层的输出。

## BFS Approach

这题最直观的就是宽度优先遍历，宽搜的精髓在于维护一个队列，每次从队头取元素，处理元素时把新元素放到队尾。这样就能保证对二叉树或者图进行遍历的时候，是齐头并进，而不是一路先钻到底。

Time complexity: O(N)

Space complexity: O(N), because we have maintain a queue which can hold up to n/2 nodes.

```java
class Solution {
  public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
      return res;
    }
    Queue<TreeNode> levelNodeQ = new LinkedList<>();
    levelNodeQ.offer(root);
    while (!levelNodeQ.isEmpty()) {
      int levelSize = levelNodeQ.size();
      List<Integer> levelList = new ArrayList<>();
      for (int i = 0; i < levelSize; i++) {
        TreeNode curr = levelNodeQ.poll();
        levelList.add(curr.val);
        if (curr.left != null) levelNodeQ.offer(curr.left);
        if (curr.right != null) levelNodeQ.offer(curr.right);
      }
      res.add(levelList);
    }
    return res;
  }
}
```