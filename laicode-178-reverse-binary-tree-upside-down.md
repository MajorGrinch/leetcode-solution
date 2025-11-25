# Laicode 178. Reverse Binary Tree Upside Down

参照[leetcode 156](156-Binary-Tree-Upside-Down.md)，只不过有细微区别。这题是父节点变成左子树，父节点的右孩子变成右子树。

```java
public class Solution {
  public TreeNode reverse(TreeNode root) {
    return helper(root, null);
  }

  private TreeNode helper(TreeNode root, TreeNode parent) {
    if(root == null) {
      return parent;
    }
    TreeNode rootNew = helper(root.left, root);
    root.left = parent;
    root.right = parent == null ? null : parent.right;
    return rootNew;
  }
}
```