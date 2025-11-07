# Laicode 299. Distance Of Two Nodes In Binary Tree

参照[leetcode 1740](1740-find-distance-in-bt.md)。

```java
public class Solution {
  public int distance(TreeNode root, int k1, int k2) {
    TreeNode lca = findLCA(root, k1, k2);
    int distance1 = getDistance(lca, k1);
    int distance2 = getDistance(lca, k2);
    return distance1 + distance2;
  }

  private int getDistance(TreeNode node, int key) {
    if(node == null) {
      return -1;
    }
    if(node.key == key) {
      return 0;
    }
    int left = getDistance(node.left, key);
    int right = getDistance(node.right, key);
    if(left == -1 && right == -1) {
      // both left and right subtree do not have the key
      return -1;
    }
    return Math.max(left, right) + 1;
  }

  private TreeNode findLCA(TreeNode node, int k1, int k2) {
    if(node == null || node.key == k1 || node.key == k2) {
      return node;
    }
    TreeNode left = findLCA(node.left, k1, k2);
    TreeNode right = findLCA(node.right, k1, k2);
    if(left != null && right != null) {
      return node;
    }
    return left == null ? right : left;
  }
}
```