# Laicode 49. Identical Binary Tree

给定两个二叉树，检查他们是否一致。

我们可以递归的去检查，分类讨论：
+ 如果根节点都为`null`，那么是一致的。
+ 如果都不为`null`，如果节点值不一样，那就是不同的。如果一样，则对左右子树分别调用并进行相同的分类讨论，并返回。
+ 如果一个是`null`，另一个不是，那显然是不一致的。

Time complexity: O(N). N is the number of nodes.

Space complexity: O(H). H is the height of the tree.

```java
public class Solution {
  public boolean isIdentical(TreeNode one, TreeNode two) {
    if (one == null && two == null) {
      return true;
    } else if (one != null && two != null) {
      if (one.key != two.key) return false;
      return isIdentical(one.left, two.left) && isIdentical(one.right, two.right);
    } else {
      return false;
    }
  }
}
```