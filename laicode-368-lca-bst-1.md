# Laicode 368. Lowest Common Ancestor Binary Search Tree I

参照[leetcode 235](235-LCA-of-BST.md)。

```java
public class Solution {
  public TreeNode lca(TreeNode root, int p, int q) {
    if(p > q) {
      return lca(root, q, p);
    }
    while(root != null) {
      if(p <= root.key && root.key <= q) {
        return root;
      } else if(root.key < p) {
        root = root.right;
      } else {
        root = root.left;
      }
    }
    return null;
  }
}
```