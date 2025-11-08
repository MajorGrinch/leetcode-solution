# Laicode 523. Flatten Binary Tree to Linked List

参照[leetcode 114](114-Flatten-Binary-Tree-to-Linked-List.md)。

```java
public class Solution {
  public TreeNode flatten(TreeNode root) {
    TreeNode[] prev = new TreeNode[] {null};
    helper(root, prev);
    return prev[0];
  }

  private void helper(TreeNode node, TreeNode[] prev) {
    if(node == null) {
      return;
    }
    helper(node.right, prev);
    helper(node.left, prev);
    node.right = prev[0];
    node.left = null;
    prev[0] = node;
  }
}
```