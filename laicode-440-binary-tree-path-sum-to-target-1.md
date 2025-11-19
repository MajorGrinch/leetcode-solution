# Laicode 440. Binary Tree Path Sum To Target I

参考[Leetcode 112](112-path-sum.md)。我们这里实现略有不同，用一个 prefixSum 记录目前为止的路径和。当前节点如果是叶子节点，那就看 prefixSum + node.key == target。当前节点如果不是叶子节点，那就累加 prefixSum，然后对左右子树继续调用。

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

2025 code:

```java
public class Solution {
  public boolean exist(TreeNode root, int target) {
    return helper(root, target, 0);
  }

  private boolean helper(TreeNode node, int target, int prefixSum) {
    if(node == null) {
      return false;
    }
    prefixSum += node.key;
    if(node.left == null && node.right == null) {
      return prefixSum == target;
    }
    
    return helper(node.left, target, prefixSum) || helper(node.right, target, prefixSum);
  }
}
```