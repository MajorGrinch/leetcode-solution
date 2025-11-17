# Laicode 504. Closest Number In Binary Search Tree II

参照[leetcode 272](272-Closest-BST-Value-II.md)。

```java
public class Solution {
  public int[] closestKValues(TreeNode root, double target, int k) {
    Queue<Integer> queue = new ArrayDeque<>();
    inorder(root, target, k, queue);
    int[] res = new int[queue.size()];
    for(int i = 0; i < res.length; i++) {
      res[i] = queue.poll();
    }
    return res;
  }

  private void inorder(TreeNode node, double target, int k, Queue<Integer> queue) {
    if(node == null) {
      return;
    }
    inorder(node.left, target, k, queue);
    if(queue.size() < k) {
      queue.offer(node.key);
    } else {
      int peekVal = queue.peek();
      if(Math.abs(node.key - target) < Math.abs(peekVal - target)) {
        // peek is further than current node
        queue.poll();
        queue.offer(node.key);
      } else {
        return;
      }
    }
    inorder(node.right, target, k, queue);
  }
}
```