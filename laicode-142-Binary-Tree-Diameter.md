# Laicode 142. Binary Tree Diameter

和[543. Diameter of Binary Tree](543-Diameter-Binary-Tree.md)差不多，只不过这里要求必须是两个叶子节点之间的路径，而且返回的是节点数而非边数。

那么其实就是多个判断当前节点是否有左右孩子，缺一个就不更新，都有的话就试图更新全局最大值。

Time complexity: O(N)

Space complexity: O(H)

```java
public class Solution {
  public int diameter(TreeNode root) {
    int[] res = {0};
    diameter(root, res);
    return res[0];
  }

  private int diameter(TreeNode root, int[] res) {
    if (root == null) {
      return 0;
    }
    int leftMaxDepth = diameter(root.left, res);
    int rightMaxDepth = diameter(root.right, res);
    if (root.left != null && root.right != null) {
      res[0] = Math.max(res[0], leftMaxDepth + rightMaxDepth + 1);
    }
    return Math.max(leftMaxDepth, rightMaxDepth) + 1;
  }
}
```