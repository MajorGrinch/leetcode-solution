# Laicode 135. Closest Number in BST

参照[leetcode 270](270-Closest-BST-Value.md)。

```java
public class Solution {
  public int closest(TreeNode root, int target) {
    int res = root.key;
    while(root != null) {
      if(Math.abs(root.key - target) < Math.abs(res - target)) {
        res = root.key;
      }
      if(root.key == target) {
        break;
      } else if(root.key < target) {
        root = root.right;
      } else {
        root = root.left;
      }
    }
    return res;
  }
}
```