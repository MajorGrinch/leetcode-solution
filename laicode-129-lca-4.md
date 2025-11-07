# Laicode 129. Lowest Common Ancestor IV

参照[Leetcode 1676](1676-LCA-of-BT-IV.md)。

```java
public class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, List<TreeNode> nodes) {
    Set<Integer> keySet = new HashSet<>();
    for(TreeNode node: nodes) {
      keySet.add(node.key);
    }
    return findLCA(root, keySet);
  }

  private TreeNode findLCA(TreeNode node, Set<Integer> keySet) {
    if(node == null || keySet.contains(node.key)) {
      return node;
    }
    TreeNode left = findLCA(node.left, keySet);
    TreeNode right = findLCA(node.right, keySet);
    if(left != null && right != null) {
      return node;
    }
    return left == null ? right : left;
  }
}
```