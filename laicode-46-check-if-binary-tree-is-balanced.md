# Laicode 46. Check If Binary Tree Is Balanced

参照[leetcode 110](110-Balanced-Binary-Tree.md)。

```java
public class Solution {
  public boolean isBalanced(TreeNode root) {
    return helper(root) != -1;
  }

  private int helper(TreeNode node) {
    if(node == null) {
      return 0;
    }
    int left = helper(node.left);
    int right = helper(node.right);
    if(left == -1 || right == -1 || Math.abs(left - right) > 1) {
      return -1;
    }
    return Math.max(left, right) + 1;
  }
}
```