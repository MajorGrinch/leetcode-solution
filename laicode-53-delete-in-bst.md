# Laicode 53. Delete in Binary Search Tree

参照[leetcode 450](450-Delete-Node-In-BST.md)的 iterative approach。

```java
public class Solution {
  public TreeNode deleteTree(TreeNode root, int key) {
    TreeNode[] found = findTarget(root, key);
    TreeNode prev = found[0];
    TreeNode keyNode = found[1];
    if(keyNode == null) {
      // not found
      return root;
    }
    if(prev == null) {
      // root is key
      return deleteBSTRoot(root);
    } else {
      if(prev.left == keyNode) {
        prev.left = deleteBSTRoot(keyNode);
      } else {
        prev.right = deleteBSTRoot(keyNode);
      }
    }
    return root;
  }

  private TreeNode[] findTarget(TreeNode root, int key) {
    TreeNode node = root;
    TreeNode prev = null;
    while(node != null && node.key != key) {
      prev = node;
      if(node.key < key) {
        node = node.right;
      } else {
        node = node.left;
      }
    }
    return new TreeNode[]{prev, node};
  }

  private TreeNode deleteBSTRoot(TreeNode root) {
    if(root == null) {
      return null;
    }
    if(root.left == null) {
      return root.right;
    } else if(root.right == null) {
      return root.left;
    }
    TreeNode next = getSmallestRight(root);
    root.key = next.key;
    root.right = deleteTree(root.right, next.key);
    return root;
  }

  private TreeNode getSmallestRight(TreeNode node) {
    node = node.right;
    while(node.left != null) {
      node = node.left;
    }
    return node;
  }
}
```