# 958. Check Completeness of a Binary Tree

验证给定的二叉树是否是完全二叉树。完全二叉树的定义是，除了最后一层其它层全是满的，且最后一层所有的节点都是靠左的。

那么我们用BFS的思想，一层一层搜。处理当前节点的时候，如果它有孩子为null，那么同一层之后所有的节点都不能有孩子。不仅仅是同一层，队列剩余的节点以及以后新加的节点都不可以有孩子，不然就不是完全二叉树了。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
  public boolean isCompleteTree(TreeNode root) {
    if (root == null) {
      return true;
    }
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    boolean metNull = false;
    while (!queue.isEmpty()) {
      TreeNode curr = queue.poll();
      if (curr.left == null) {
        metNull = true;
      } else {
        if (metNull) {
          return false;
        }
        queue.offer(curr.left);
      }
      if (curr.right == null) {
        metNull = true;
      } else {
        if (metNull) {
          return false;
        }
        queue.offer(curr.right);
      }
    }
    return true;
  }
}
```