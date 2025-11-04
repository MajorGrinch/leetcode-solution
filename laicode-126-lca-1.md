# Laicode 126. Lowest Common Ancestor I

参照[Leetcode 236](236-LCA.md)。


```java
public class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode one, TreeNode two) {
    if(root == null || root == one || root == two) {
      return root;
    }
    TreeNode left = lowestCommonAncestor(root.left, one, two);
    TreeNode right = lowestCommonAncestor(root.right, one, two);
    if(left != null && right != null) {
      return root;
    }
    return left == null ? right: left;
  }
}
```