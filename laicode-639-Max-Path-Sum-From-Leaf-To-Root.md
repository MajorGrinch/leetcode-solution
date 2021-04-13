# Laicode 639. Max Path Sum From Leaf To Root

这题计算从root到叶子节点的最大路径和。

这次我们不再采用从底向上的返回形式了，而是采用自上而下的遍历形式。途中我们要记录目前为止从root到上一层节点的路径和，并且传入当前层作为参数以供调用。当前节点是叶子节点时候，就尝试更新答案。如果不是叶子节点，那就对左右子树进行相同的调用直到碰到叶子节点为止。

Time complexity: O(N).

Space complexity: O(H).

```java
public class Solution {
  public int maxPathSumLeafToRoot(TreeNode root) {
    int[] res = new int[] {Integer.MIN_VALUE};
    maxPathSumLeafToRoot(root, 0, res);
    return res[0];
  }

  private void maxPathSumLeafToRoot(TreeNode root, int prefixSum, int[] res) {
    if (root == null) {
      return;
    }

    if (isLeaf(root)) {
      res[0] = Math.max(prefixSum + root.key, res[0]);
      return;
    }
    maxPathSumLeafToRoot(root.left, prefixSum + root.key, res);
    maxPathSumLeafToRoot(root.right, prefixSum + root.key, res);
  }

  private boolean isLeaf(TreeNode node) {
    return node.left == null && node.right == null;
  }
}
```