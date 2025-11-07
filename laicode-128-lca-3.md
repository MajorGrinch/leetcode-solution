# Laicode 128. Lowest Common Ancestor III

参照[leetcode 1644](1644-LCA-II.md)。

```java
public class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode one, TreeNode two) {
    TreeNode lca = findLCA(root, one, two);
    if(lca == one) {
      return findLCA(one, two, two) == null ? null : one;
    } else if(lca == two) {
      return findLCA(two, one, one) == null ? null : two;
    } else {
      return lca;
    }
  }

  private TreeNode findLCA(TreeNode node, TreeNode one, TreeNode two) {
    if(node == null || node == one || node == two) {
      return node;
    }
    TreeNode left = findLCA(node.left, one, two);
    TreeNode right = findLCA(node.right, one, two);
    if(left != null && right != null) {
      return node;
    }
    return left == null ? right : left;
  }
}
```