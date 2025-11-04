# Laicode 440. Binary Tree Path Sum To Target I

参考[Leetcode 112](112-path-sum.md)。

```java
public class Solution {
  public boolean exist(TreeNode root, int target) {
    if(root == null) {
      return false;
    }
    return helper(root, 0, target);
  }

  private boolean helper(TreeNode node, int currSum, int target) {
    if(node.left == null && node.right == null) {
      return currSum + node.key == target;
    }
    currSum += node.key;
    boolean res = false;
    if(node.left != null) {
      res |= helper(node.left, currSum, target);
    }
    if(node.right != null) {
      res |= helper(node.right, currSum, target);
    }
    return res;
  }
}
```