# 270. Closest Binary Search Tree Value

给定一个BST和一个target值，找到离`target`最近的值。

那么其实就是在二叉树里找`target`，如果能找到那就返回这个值。如果找不到，在途中记录离`target`最近的值最后返回就好。

Time complexity: O(H). H is the height.

Space complexity: O(1).

```java
class Solution {
  public int closestValue(TreeNode root, double target) {
    TreeNode curr = root;
    int res = curr.val;
    while (curr != null) {
      if (curr.val == target) {
        return curr.val;
      } else {
        if (Math.abs(res - target) > Math.abs(curr.val - target)) {
          res = curr.val;
        }
        if (curr.val < target) curr = curr.right;
        else curr = curr.left;
      }
    }
    return res;
  }
}
```