# 1644. Lowest Common Ancestor of a Binary Tree II

给定一个二叉树和两个节点，寻找他们的最近公共祖先。

Clarification:
+ 二叉树所有节点都是独一无二的
+ 给定的点不同
+ 给定的两个点不一定在树里，如果不存在的话返回`null`

那么这题我们还是和普通的找LCA一样，先找到LCA。接着我们需要判断找到的LCA是否是输入节点之一，如果是的话就以那个节点为根去看看另一个节点是否在那个子树里，如果不在的话那就说明另一个节点根本不在这整个树里。怎么判断是否在子树里呢？直接用相同的找LCA方法，只是这次两个节点都一样，这样就可以复用我们的方法来判断该节点是否在子树里。

Time complexity: O(N)

Space complexity: O(H)

```java
class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    TreeNode lca = getLCA(root, p, q);
    if (lca == p) {
      if (getLCA(p, q, q) == null) return null;
    } else if (lca == q) {
      if (getLCA(q, p, p) == null) return null;
    }
    return lca;
  }

  private TreeNode getLCA(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) {
      return root;
    }

    TreeNode leftRes = getLCA(root.left, p, q);
    TreeNode rightRes = getLCA(root.right, p, q);

    if (leftRes != null && rightRes != null) {
      return root;
    }
    return leftRes != null ? leftRes : rightRes;
  }
}
```