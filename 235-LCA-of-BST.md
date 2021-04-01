# 235. Lowest Common Ancestor of a Binary Search Tree

给一个二叉搜索树，找出两个节点的最近公共祖先。

二叉搜索树找两个节点的最近公共祖先其实很好找，找到那个比其中一个小以及比另外一个大的节点就行，该节点就是两个节点的最近公共祖先。

Time complexity: O(H), where H is the height.

Space complexity: O(H), where H is the height.

```java
class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) {
      return null;
    }
    if (p.val > q.val) {
      return lowestCommonAncestor(root, q, p);
    }
    TreeNode curr = root;
    while (curr != null) {
      if (curr.val < p.val) {
        curr = curr.right;
      } else if (curr.val > q.val) {
        curr = curr.left;
      } else {
        break;
      }
    }
    return curr;
  }
}
```