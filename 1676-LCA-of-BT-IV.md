# 1676. Lowest Common Ancestor of a Binary Tree IV

给定一个二叉树和一个数组的节点，找到他们的最近公共祖先。

其实思路还是一样的，以前是找两个，现在是找多个而已。用一个Set来存放所有的节点，然后只要当前节点在Set里就直接返回。如果左右子树都有找到Set里的节点，那么返回当前节点。否则返回左右子树查询结果里非空的那个，如果都空就返回空。

Time complexity: O(N).

Space complexity: O(H). H is the height.

```java
class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
    Set<TreeNode> nodeSet = new HashSet<>();
    for (TreeNode node : nodes) nodeSet.add(node);
    return LCA(root, nodeSet);
  }

  private TreeNode LCA(TreeNode root, Set<TreeNode> nodeSet) {
    if (root == null || nodeSet.contains(root)) {
      return root;
    }
    TreeNode leftRes = LCA(root.left, nodeSet);
    TreeNode rightRes = LCA(root.right, nodeSet);
    if (leftRes != null && rightRes != null) {
      return root;
    }
    return leftRes != null ? leftRes : rightRes;
  }
}
```

2025 code:

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
        Set<Integer> valSet = new HashSet<>();
        for (TreeNode n : nodes) {
            valSet.add(n.val);
        }
        return findLCA(root, valSet);
    }

    private TreeNode findLCA(TreeNode node, Set<Integer> valSet) {
        if (node == null || valSet.contains(node.val)) {
            return node;
        }
        TreeNode left = findLCA(node.left, valSet);
        TreeNode right = findLCA(node.right, valSet);
        if (left != null && right != null) {
            return node;
        }
        return left == null ? right : left;
    }
}
```