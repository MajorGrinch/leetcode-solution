# Laicode 139. Maximum Path Sum Binary Tree II

直接参照[leetcode 124](124-Binary-Tree-Max-Path-Sum.md)，一模一样。


```java
public class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = {Integer.MIN_VALUE};
    maxPathSum(root, res);
    return res[0];
  }

  private int maxPathSum(TreeNode node, int[] res) {
    if(node == null) {
      return 0;
    }
    int leftSum = maxPathSum(node.left, res);
    leftSum = Math.max(leftSum, 0);
    int rightSum = maxPathSum(node.right, res);
    rightSum = Math.max(rightSum, 0);
    // update result
    res[0] = Math.max(res[0], leftSum + rightSum + node.key);
    return Math.max(leftSum, rightSum) + node.key;
  }
}
```

也可以这样写

```java
public class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = {Integer.MIN_VALUE};
    helper(root, res);
    return res[0];
  }

  private int helper(TreeNode node, int[] res) {
    if(node == null) {
      return 0;
    }
    int left = Math.max(0, helper(node.left, res));
    int right = Math.max(0, helper(node.right, res));
    res[0] = Math.max(res[0], left + right + node.key);
    return Math.max(left, right) + node.key;
  }
}
```