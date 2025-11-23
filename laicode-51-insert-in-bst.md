# Laicode 51. Insert in Binary Search Tree

参照[leetcode 701](701-Insert-Into-BST.md)。

```java
public class Solution {
  public TreeNode insert(TreeNode root, int key) {
    if(root == null) {
      return new TreeNode(key);
    }
    
    TreeNode curr = root;
    TreeNode parent = null;
    while(curr != null) {
      parent = curr;
      if(curr.key == key) {
        return root;
      } else if(curr.key < key) {
        curr = curr.right;
      } else {
        curr = curr.left;
      }
    }
    if(key < parent.key) {
      parent.left = new TreeNode(key);
    } else {
      parent.right = new TreeNode(key);
    }
    return root;
  }
}
```