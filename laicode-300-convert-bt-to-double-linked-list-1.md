# Laicode 300. Convert Binary Tree To Doubly Linked List I

参照[leetcode 114](114-Flatten-Binary-Tree-to-Linked-List.md)，只不过这题是按照中序遍历的顺序。那么我们就反中序遍历，按照右根左的顺序，来建立双端链表。

```java
public class Solution {
  public TreeNode toDoubleLinkedList(TreeNode root) {
    TreeNode[] prev = {null};
    helper(root, prev);
    return prev[0];
  }

  private void helper(TreeNode node, TreeNode[] prev) {
    if(node == null) {
      return;
    }
    helper(node.right, prev);
    node.right = prev[0];
    if(node.right != null) {
      node.right.left = node;
    }
    prev[0] = node;
    helper(node.left, prev);
  }
}
```