# Laicode 481. Count Univalue Subtrees

参照[Leetcode 250](250-count-univalue-subtree.md)。

```java
public class Solution {
  public int countUnivalSubtrees(TreeNode root) {
    int[] res = {0};
    helper(root, res);
    return res[0];
  }

  private boolean helper(TreeNode node, int[] res) {
    if(node == null) {
      return true;
    }
    boolean left = helper(node.left, res);
    boolean right = helper(node.right, res);
    boolean isUni = left && right;
    if(node.left != null) {
      isUni &= node.left.key == node.key;
    }
    if(node.right != null) {
      isUni &= node.right.key == node.key;
    }
    if(isUni) {
      res[0]++;
    }
    return isUni;
  }
}
```